<template>
	<view class="bigest">

		<div class="box1" v-for="(item,index) in form" :key="index">

			<h3>教育信息<text v-text="form.length>1?index+1:'' "> </text>
				<a v-show="form.length>1" style="margin-left: 15px;" href="javaScript:;" class="r" :index="index"
					@click="remove(index)">删除</a>
				<a v-show="form.length>1" href="javaScript:;" class="b" @click="zhankai(index)"
					v-text="item.flag?'收起':'展开' "></a>
			</h3>

			<u-form :model="item" ref="uForm" label-align="left" label-width="75px" :label-style="label_style"
				v-show="item.flag">

				<u-form-item label="开始时间" required prop="startTime">
					<u-input v-model="item.startTime" @click="show1=!show1" placeholder="请输入" type="select" />
					<u-calendar v-model="show1" mode="date" @change="change1($event,index)"></u-calendar>
				</u-form-item>

				<u-form-item label="结束时间" required prop="endTime">
					<u-input v-model="item.endTime" @click="show2=!show2" placeholder="请输入" type="select" />
					<u-calendar v-model="show2" :mode="mode" @change="change2($event,index)"></u-calendar>
				</u-form-item>

				<u-form-item label="院校名称" prop="school" required>
					<u-input v-model="item.school" placeholder="请输入" />
				</u-form-item>


				<u-form-item label="学历" prop="education" required>
					<u-input v-model="item.education" @click="changescholar" placeholder="请输入" type="select" />
					<u-select v-model="showscholar" mode="single-column" :list="list" @confirm="confirm($event,index)">
					</u-select>
				</u-form-item>

				<u-form-item label="学习方式" prop="studyWay" required>
					<u-input v-model="item.studyWay" placeholder="请输入" @click="changemethod" type="select" />
					<u-select v-model="showmethod" mode="single-column" :list="list1" @confirm="confirm1($event,index)">
					</u-select>
				</u-form-item>

				<u-form-item label="学制" prop="schoolSystem" required>
					<u-input v-model="item.schoolSystem" placeholder="请输入" />
				</u-form-item>

				<u-form-item label="毕业类型" prop="graduationType" required>
					<u-input v-model="item.graduationType" @click="graduate" placeholder="请输入" type="select" />
					<u-select v-model="graduatetp" mode="single-column" :list="list2" @confirm="confirm2($event,index)">
					</u-select>
				</u-form-item>

				<u-form-item label="是否最高学历" prop="highestDegree" required label-width="100px">
					<u-input v-model="item.highestDegree" @click="higestxlone" placeholder="请输入" type="select" />
					<u-select v-model="higestxl" mode="single-column" :list="list3" @confirm="confirm3($event,index)">
					</u-select>

				</u-form-item>

				<u-form-item label="是否第一学历" prop="firstDegree" required label-width="100px">
					<u-input v-model="item.firstDegree" @click="firstxl=!firstxl" placeholder="请输入" type="select" />
					<u-select v-model="firstxl" mode="single-column" :list="list5" @confirm="confirm5($event,index)">
					</u-select>
				</u-form-item>

				<u-form-item label="是否最高学位" prop="highestScience" required label-width="100px">
					<u-input v-model="item.highestScience" @click="higestxw=!higestxw" placeholder="请输入"
						type="select" />
					<u-select v-model="higestxw" mode="single-column" :list="list6" @confirm="confirm6($event,index)">
					</u-select>
				</u-form-item>

				<u-form-item label="学校类型" prop="schooltp" required>
					<u-input v-model="item.schoolType" @click="schooltp=!schooltp" placeholder="请输入" type="select" />
					<u-select v-model="schooltp" mode="single-column" :list="list7" @confirm="confirm7($event,index)">
					</u-select>
				</u-form-item>

				<u-form-item label="学位" prop="xw" required>
					<u-input v-model="item.science" @click="xw=!xw" placeholder="请输入" type="select" />
					<u-select v-model="xw" mode="single-column" :list="list8" @confirm="confirm8($event,index)">
					</u-select>
				</u-form-item>
			</u-form>
		</div>

		<div class="add" @click="submit">+ 新增一条教育信息</div>
		<u-button class="next" @click="nextStep">下一步</u-button>
	</view>
</template>

<script>
	export default {
		created() {
			this.getben();
		},
		mounted() {

			this.$refs.uForm[0].setRules(this.rules);
			// uni.getStorage({
			// 	key: 'basicInformation',
			// 	success: res=> {
			// 		this.basicInformation = res.data
			// 	},
			// }),					
		},

		data() {
			return {
				startTimeCheck: true,
				endTimeCheck: true,
				fl: false,
				can: {
					"basicInformation": {
						"accountType": "城市",
						"clan": "汉族",
						"contactInformation": 13690631123,
						"countryOfCitizenship": "中国",
						"dateOfBirth": "1997-06-06",
						"email": "456123456@qq.com",
						"emergencyContact": "王五",
						"englishName": "Mike",
						"formerName": "李四",
						"fullNameSpelling": "ZhangSan",
						"graduates": 0,
						"hometown": "湖南长沙",
						"id": parseInt(Math.random() * 100000),
						"idNumber": parseInt(Math.random() * 100000),
						"maritalStatus": "未婚",
						"name": "张三",
						"phone": 13690631774,
						"sex": "男",
						"specialHobby": "跑步",
						"typeOfCertificate": "身份证"
					},
				},

				label_style: {
					fontSize: "30rpx",
					fontWeight: 'bold'
				},
				graduatetp: false,
				show1: false,
				show2: false,
				mode: "date",
				firstxl: false,
				higestxw: false,
				xw: false,
				form: [{
					startTime: '',
					endTime: "",
					school: "",
					education: "",
					studyWay: "",
					schoolSystem: "",
					graduationType: "",
					highestxl: "",
					firstDegree: "",
					highestScience: "",
					schoolType: "",
					flag: true,
					science: "",
				}],
				showscholar: false,
				showmethod: false,
				higestxl: false,
				schooltp: false,
				list: [{
						value: '1',
						label: '小学'
					},
					{
						value: '2',
						label: '初级中学'
					},
					{
						value: '3',
						label: '高级中学'
					},
					{
						value: '4',
						label: '大学专科'
					},
					{
						value: '5',
						label: '大学本科'
					},
					{
						value: '6',
						label: '硕士研究生'
					},
					{
						value: '7',
						label: '博士研究生'
					}
				],


				list1: [{
					value: '1',
					label: "全日制"
				}, {
					value: "2",
					label: "半日制"
				}],

				list2: [{
						value: '1',
						label: "就页类"
					}, {
						value: "2",
						label: "创业类"
					},
					{
						value: "3",
						label: "学业类"
					},
					{
						value: "4",
						label: "待就业"
					},
				],
				list3: [{
						value: "1",
						label: "是"
					},
					{
						value: "2",
						label: "否"
					},
				],
				list5: [{
						value: "1",
						label: "是"
					},
					{
						value: "2",
						label: "否"
					},
				],
				list6: [{
						value: "1",
						label: "是"
					},
					{
						value: "2",
						label: "否"
					},
				],
				list7: [{
						value: "1",
						label: "全日制学校"
					},
					{
						value: "2",
						label: "半工(农) 半读学校"
					},
				],
				list8: [{
						value: "1",
						label: "学士"
					},
					{
						value: "2",
						label: "硕士"
					},
					{
						value: "3",
						label: "博士"
					},
				],
				rules: {
					startTime: [{
						required: true,
						message: "请输入开始时间",
						trigger: ['change', 'blur'],
					}],
					endTime: [{
						required: true,
						message: "请输入结束时间",
						trigger: ['change', 'blur'],
					}],
					school: [{
						required: true,
						message: "请输入学校名称",
						trigger: ['change', 'blur'],
					}],
					education: [{
						required: true,
						message: "请输入学历",
						trigger: ['change', 'blur'],
					}],
					studyWay: [{
						required: true,
						message: "请输入学习方式",
						trigger: ['change', 'blur'],
					}],
					schoolSystem: [{
						required: true,
						message: "请输入学制",
						trigger: ['change', 'blur'],
					}],
					graduationType: [{
						required: true,
						message: "请输入毕业类型",
						trigger: ['change', 'blur'],
					}],
					highestDegree: [{
						required: true,
						message: "是否是最高学历",
						trigger: ['change', 'blur'],
					}],
					firstDegree: [{
						required: true,
						message: "是否是第一学历",
						trigger: ['change', 'blur'],
					}],
					highestScience: [{
						required: true,
						message: "是否是最高学位",
						trigger: ['change', 'blur'],
					}],
					schooltp: [{
						required: true,
						message: "请输入学校类型",
						trigger: ['change', 'blur'],
					}],
					xw: [{
						required: true,
						message: "请输入学位",
						trigger: ['change', 'blur'],
					}]
				}
			};
		},
		methods: {
			change1(e, index) {
				if (this.form.length > 1) {
					for (var i = 0; i < this.form.length; i++) {
						if (new Date(e.result).getTime() >= new Date(this.form[i].startTime).getTime() && new Date(e
								.result).getTime() <= new Date(this.form[i].endTime).getTime()) {
							console.log("改变为false")
							this.startTimeCheck = false;
							break;
						} else {
							if (i == this.form.length - 1) {
								this.startTimeCheck = true;
								break;
							}
						}
					}

					if (this.startTimeCheck) {
						this.form[index].startTime = e.result;
					} else {
						alert("时间有冲突，请输入其它时间");
						this.form[index].startTime = "";
					}
				} //第一次开始时间以后的，都得在其他的开始时间到结束时间的范围之外
				else {
					if (this.form[index].endTime) {
						if (new Date(e.result).getTime() < new Date(this.form[index].endTime).getTime()) {
							this.form[index].startTime = e.result;
						} else {
							alert("开始时间必须小于结束时间");
							this.form[index].startTime = "";
						}
					} else {
						this.form[index].startTime = e.result;
					}
				} //如果是第一次输入，先检测有没有填结束时间，如果填了，那么在修改开始时间时，就得让开始时间必须小于结束时间
			},

			change2(e, index) {

			},
			//若数组长度为1，结束时间，只需大于第一次的开始时间即可
			//若数组长度不为1，结束时间，首先，得大于开始时间 ，然后，若此时的开始时间大于等于其他的开始时间 且 此时的结束时间小于等于其他的结束时间，只要出现一次
			//，认定为为时间冲突,便让endTimeCheck为false,此时e.result就不赋上去了,如果这个值为合理值，就让startTimeCheck为true					
			askHou(can) {
				uni.request({
					url: "http://192.168.8.120:10001/onboarding/basicinformation/savePerfectPersonalInformation",
					method: "POST",
					data: can,
					success: function(res) {
						console.log("向后台发送教育数据", res);
					}
				})
			},
			bencun(can1) {
				uni.setStorage({
					key: "education-info",
					data: can1
				})
			},
			getben() {
				uni.getStorage({
					key: "education-info",
					success: function(res) {
						this.form = res.data;
					}
				})
			},
			//添加
			zhankai(index) {

				for (var i = 0; i < this.form.length; i++) {
					if (i == index) {
						this.form[index].flag = !this.form[index].flag;
						continue;
					}
					this.form[i].flag = false;
				}

			},
			loop() {
				for (var i = 0; i < this.form.length; i++) {
					this.form[i].flag = false;
				}
				this.form[this.form.length - 1].flag = true;
			},
			submit() {
				// this.setStorage();
				if (this.form.length !== 0) {
					this.$refs.uForm[0].validate(valid => {
						if (valid) {} else {}
						this.form.push({
							startTime: '',
							endTime: "",
							school: "",
							education: "",
							studyWay: "",
							schoolSystem: "",
							graduationType: "",
							highestxl: "",
							firstDegree: "",
							highestScience: "",
							schoolType: "",
							science: "",
							flag: true
						});
						this.loop();
					});
				} else {
					this.form.push({
						startTime: '',
						endTime: "",
						school: "",
						education: "",
						studyWay: "",
						schoolSystem: "",
						graduationType: "",
						highestxl: "",
						firstDegree: "",
						highestScience: "",
						schoolType: "",
						science: "",
					});
					this.loop();
				}
			},
			//添加结束


			higestxlone() {
				this.higestxl = !this.higestxl;
			},
			changemethod() {
				this.showmethod = !this.showmethod;
			},
			graduate() {
				this.graduatetp = !this.graduatetp;
			},
			changescholar() {
				this.showscholar = !this.showscholar;
			},
			confirm(e, index) {

				this.form[index].education = e[0].label;
			},
			confirm1(e, index) {
				this.form[index].studyWay = e[0].label;
			},
			confirm2(e, index) {
				this.form[index].graduationType = e[0].label;
			},
			confirm3(e, index) {
				if (this.fl) {
					this.form[index].highestDegree = "否";
				} else {
					this.form[index].highestDegree = e[0].label;
				}

				for (var i = 0; i < this.form.length; i++) {

					if (this.form[i].highestDegree == "是") {
						this.fl = true;
						console.log("上", index, this.fl);
						break;
					} else {
						this.fl = false;
					}
				}
				console.log("下", index, this.fl);


			},
			confirm5(e, index) {
				this.form[index].firstDegree = e[0].label;
			},
			confirm6(e, index) {
				this.form[index].highestScience = e[0].label;
			},
			confirm7(e, index) {
				this.form[index].schoolType = e[0].label;
			},
			confirm8(e, index) {
				this.form[index].science = e[0].label;
			},
			remove(index) {
				if (this.form[index].highestDegree == "是") {
					this.fl = false;
				}
				// else{
				// 	this.fl=true;
				// }

				this.form.splice(index, 1);
				this.bencun(this.form);
				if (this.form.length == 1) {
					this.form[0].flag = true;
				}
			},
			nextStep() {
				this.can["educationalInformation"] = this.form;
				this.bencun(this.form);
				this.askHou(this.form);
				this.askHou(this.can);
			}
		},
		onReady() {
			this.$refs.uForm[0].setRules(this.rules);
		},

	}
</script>

<style lang="scss" scoped>
	h3 {
		font-size: 17px;
		padding: 15px 0;
		font-weight: 560;
		border-bottom: 1px solid #e4e7ed;

		a {
			color: #499CF2;
			float: right;
			text-decoration: none;
			font-weight: normal;
		}

		a:nth-child(2) {
			margin-right: 30px;
		}

	}



	page {
		padding: 0px 15px;
		background-color: #FAFAFA;
	}

	.icon-xinghao {
		color: red;
	}

	.add {
		margin-top: 30px !important;
		width: 100%;
		height: 40px;
		text-align: center;
		margin: auto;
		line-height: 40px;
		font-size: 17px;
		border: 1px dashed grey;
		font-family: "iconfont";
		font-size: 20px;
	}

	.next {
		margin-top: 50px !important;
		background-color: #474747;
		height: 50px;
		color: white;
	}

	.bigest {
		// border: 10px solid blue;
	}
</style>
