<template>

	<view class="bigest">
		<view class="box1" v-for="(item,index) in form" :key="index">
			<h3>家庭信息<text v-text="form.length>1?index+1:'' "> </text>
				<a style="margin-left: 15px;color:#499CF2" v-show="form.length>1" href="javaScript:;" class="b"
					@click="zhankai(index)" v-text="item.flag?'收起':'展开' "></a>
				<a style="color:#499CF2" v-show="form.length>1" href="javaScript:;" class="r" :index="index"
					@click="remove(index)">删除</a>
			</h3>

			<u-form :model="item" ref="uForm" label-align="left" label-width="70px" :label-style="label_style"
				v-show="item.flag">
				<u-form-item label="姓名" prop="name" required>
					<u-input required v-model="item.name" placeholder="请输入" />
				</u-form-item>

				<u-form-item label="关系" required prop="relationship" style="">
					<u-input required type="select" v-model="item.relationship" @click="toggleRel" placeholder="请输入"
						style="" />
					<u-select v-model="showRel" mode="single-column" :list="list" @confirm="confirm($event,index)">
					</u-select>
				</u-form-item>

				<u-form-item label="出生日期" prop="dateOfBirth">
					<u-input v-model="item.dateOfBirth" type="select" required @click="togglebir" placeholder="请输入"
						style="" />
					<u-calendar v-model="showbir" mode="date" @change="change($event,index)" value="123"></u-calendar>
				</u-form-item>


				<u-form-item label="工作单位" prop="employer">
					<u-input v-model="item.employer" placeholder="请输入" style="" />
				</u-form-item>

				<u-form-item label="联系地址" prop="contactAddress">
					<u-input v-model="item.contactAddress" placeholder="请输入" style=" " />
				</u-form-item>

				<u-form-item label="联系电话" required prop="phone">
					<u-input v-model="item.phone" placeholder="请输入" style="" />
				</u-form-item>

			</u-form>
		</view>

		<div class="add" @click="submit">+ 新增一条家庭信息</div>
		<u-button class="next" @click="nextStep">下一步</u-button>
	</view>
</template>

<script>
	export default {
		data() {
			return {       
				label_style: {
					fontSize: "30rpx",
					fontWeight: 'bold'
				},
				form: [{
					name: '',
					relationship: '',
					dateOfBirth: '',
					employer: "",
					contactAddress: "",
					phone: "",
					flag: true
				}],
				showRel: false,
				showbir: false,
				listCheck:[false,false,false,false,false,false,false,false,false],
				list: [{
						value: '1',
						label: '本人'
					},
					{
						value: '2',
						label: '父亲'
					}, {
						value: "3",
						label: "母亲"
					}, {
						value: '4',
						label: "弟弟"
					}, {
						value: '5',
						label: "妹妹",
					}, {
						value: "6",
						label: "哥哥"
					}, {
						value: "7",
						label: "姐姐"
					},
					{
						value: "8",
						label: "爷爷"
					}, {
						value: "9",
						label: "奶奶"
					}
				],
				rules: {
					name: [{
						required: true,
						message: "请输入姓名",
						trigger: ['change', 'blur'],
					}],
					relationship: [{
						required: true,
						message: "请输入关系",
						trigger: ['change', 'blur'],
					}],
					dateOfBirth: [{
						required: true,
						message: "请输入出生日期",
						trigger: ['change', 'blur'],
					}],
					employer: [{
						message: "请输入工作单位",
						trigger: ['change', 'blur'],
					}],
					contactAddress: [{
						message: "请输入联系地址",
						trigger: ['change', 'blur'],
					}],
					phone: [{
							required: true,
							message: "请输入联系电话",
							trigger: ['change', 'blur'],
						},
						{
							validator: function(rule, value, callback) {
								if (/^1[34578]\d{9}$/.test(value) == false) {
									callback(new Error("号码格式错误"));
								} else {
									callback();
								}
							},
							trigger: "blur"
						}
					]
				}
			};
		},
		created() {
			uni.getStorage({
				key: "family-data",
				success: function(res) {
					this.form = res.data;
				}
			});

		},
		mounted() {
			this.$refs.uForm[0].setRules(this.rules);
		},


		methods: {
			askHou(can) {
				uni.request({
					url: "http://192.168.8.120:10001/onboarding/basicinformation/savePerfectPersonalInformation",
					method: "POST",
					data: can,
					success: function(res) {
						console.log("向后台发送家庭数据", res);
					}
				})
			},

			ask(can) {
				uni.request({
					url: "http://192.168.8.120:10001/onboarding/basicinformation/savePerfectPersonalInformation",
					method: "POST",
					data: can,
					success: function(res) {
						// console.log(100,res);
					}
				})
			},

			zhankai(index) {
				// console.log(index);
				// console.log(this.form[index].flag);
				for (var i = 0; i < this.form.length; i++) {
					if (i == index) {
						this.form[index].flag = !this.form[index].flag;
						continue;
					}
					this.form[i].flag = false;
				}
			},
			confirm(e, index) {
				
				
				for(var i=0;i<this.form.length;i++){
					for(var j=0;j<this.list.length;j++){
						if(this.form[i].relationship==this.list[j].label){
							   this.listCheck[j]=true;
						}
					}
				}
				
				for(var i=0;i<this.list.length;i++){
					if(this.list[i].label==this.form[index].relationship){
						var num=parseInt(this.list[i].value)-1;
						this.listCheck[num]=false;
						break;
					}
				}//
				
				
				
				var num=parseInt(e[0].value)-1;
				// console.log(this.list[num]);
				console.log("bug",this.listCheck[num]);
				if(this.listCheck[num]==true){
					alert("该项已经被选择过,请选择其它项");
					this.form[index].relationship = "";	
				}else{
					this.form[index].relationship=e[0].label;
				}
			},

			loop() {
				for (var i = 0; i < this.form.length; i++) {
					this.form[i].flag = false;
					this.form[this.form.length - 1].flag = true;
				}
			},

			getben() {
				uni.getStorage({
					key: "family-info",
					success: function(res) {
						this.form = res.data;
					}
				})
			},

			submit() {
				if (this.form.length !== 0) {
					this.$refs.uForm[0].validate(valid => {
						if (valid) {} else {}
						this.form.push({
							name: '',
							relationship: '',
							dateOfBirth: '',
							employer: "",
							contactAddress: "",
							phone: "",
							flag: true
						});
						this.loop();
					});
				} else {
					this.form.push({
						name: '',
						relationship: '',
						dateOfBirth: '',
						employer: "",
						contactAddress: "",
						phone: "",
						flag: true
					});
					this.loop();
				}
			},
			toggleRel() {
				this.showRel = !this.showRel;
			},
			togglebir() {
				this.showbir = !this.showbir;
			},
			change(e, index) {
				this.form[index].dateOfBirth = e.result;
			},
			remove(index) {
				for(var i=0;i<this.list.length;i++){
					if(this.list[i].label==this.form[index].relationship){
						var num=parseInt(this.list[i].value)-1;
						this.listCheck[num]=false;
						break;
					}
				}
				
				this.form.splice(index, 1);
				if (this.form.length == 1) {
					this.form[0].flag = true;
				}
			},
			nextStep() {
				this.askHou(this.form);
				this.bencun(this.form);
				// this.$emit('nextStep')
			},
			bencun(can1) {

				uni.setStorage({
					key: "family-info",
					data: can1
				})
			},
		},
		onReady() {
			alert(1);

		},
	};
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
		margin-top: 70px !important;
		background-color: #474747;
		height: 50px;
		color: white;
	}

	.bigest {
		// border: 10px solid blue;
	}

	.box1 {
		// margin-bottom: 20px;
	}

	.isHidden {
		display: none;
	}

	.isShow {
		display: block;
	}

	.r {
		color: red;
	}

	.b {
		color: black;
	}
</style>
