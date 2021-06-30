<template>
	<view class="bigest">
		
	<div class="box1"  v-for="(item,index) in form">
		
		<h3>培训信息<text v-text="form.length>1?index+1:'' "> </text>
		<a   style="margin-left: 15px;" v-show="form.length>1"  href="javaScript:;"	class="b" @click="zhankai(index)" v-text="item.flag?'收起':'展开' "></a>
		<a   v-show="form.length>1"  href="javaScript:;" class="r"   @click="remove(index)">删除</a>
		</h3>
		
		<u-form :model="item" ref="uForm" label-align="left" label-width="75px" v-show="item.flag" :label-style="label_style">
			
			<u-form-item required label="开始时间"  prop="startTime" >
				<u-input v-model="item.startTime"  @click="show1=!show1" placeholder="请输入" type="select"  />
				<u-calendar v-model="show1" mode="date" @change="change1($event,index)"></u-calendar>
				</u-form-item>
				
      <u-form-item required label="结束时间"  prop="stopTime" >
				<u-input v-model="item.stopTime" @click="show2=!show2"   placeholder="请输入" type="select"  />
		  <u-calendar v-model="show2" mode="date" @change="change2($event,index)"></u-calendar>		
			</u-form-item>
    
			<u-form-item required label="培训课程"  prop="train" >
			<u-input v-model="item.train"  placeholder="请输入" />
     </u-form-item>
			
			
      <u-form-item required  label="获得证书"   prop="certificates"  >
				<u-input v-model="item.certificates" placeholder="请输入" />
				</u-form-item>
      
			<u-form-item required label="职称"   prop="jobTitle" label-align="left" >
				<u-input v-model="item.jobTitle"  placeholder="请输入" />
			</u-form-item>
     
	</u-form>
	</div>
	
      <div  class="add" @click="submit">+ 新增一条教育信息</div>
			<u-button class="next"  @click="nextStep">下一步</u-button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				can:{
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
					    "id": parseInt(Math.random()*100000),
					    "idNumber": parseInt(Math.random()*100000),
					    "maritalStatus": "未婚",
					    "name": "张三",
					    "phone": 13690631774,
					    "sex": "男",
					    "specialHobby": "跑步",
					    "typeOfCertificate": "身份证"
					  },
				},
				label_style: { fontSize: "30rpx",fontWeight: 'bold'},
			   listObject:[],
				index:1,
				newObj:null,
				show1:false,
				show2:false,
				form: [{
					startTime: '',
					stopTime: '',
					train: '',
					certificates:"",
					jobTitle:"",
							flag:true
				}],
				// show:false,
				// mode: 'date',
				rules:{
					startTime:[{
						required:true,
						message:"请输入开始时间",
						trigger: ['change','blur'],						
					}						
					],
							stopTime:[{
								required:true,
								message:"请输入结束时间",
								trigger: ['change','blur'],						
							}],
								train:[{
									required:true,
									message:"请输入培训课程",
									trigger: ['change','blur'],						
								}],
								certificates:[{
									required:true,
									message:"请输入获得证书",
									trigger: ['change','blur'],						
								}],
								jobTitle:[{		
									required:true,
									message:"请输入职称",
									trigger: ['change','blur'],						
								}]
				}			
			};
		},
		methods:{
			askHou(can){
				uni.request({
					url:"http://192.168.8.120:10001/onboarding/basicinformation/savePerfectPersonalInformation",
					method:"POST",
					data: can	,	
					success:function(res){
						console.log("向后台发送培训数据",res);
					}
				})
			},	
			bencun(can1){
										 uni.setStorage({
											 key:"training-info",
											 data:can1
										 })
			},
			getben(){
										 uni.getStorage({
											 key:"training-info",
											 success:function(res){
												 this.form=res.data;
												 // console.log(this.form);
											 }
										 })
			},
			change1(e,index) {
				console.log(e.result);
					this.form[index].startTime=e.result;
				},
				change2(e,index){
							this.form[index].stopTime=e.result;
				},
			remove(index){
				this.form.splice(index,1);
								if(this.form.length==1){
									this.form[0].flag=true;
								}
			},
			zhankai(index) {
			    console.log(index);
					console.log(this.form[index].flag);
				for (var i = 0; i < this.form.length; i++) {
					if(i==index){
							this.form[index].flag =!this.form[index].flag ;
						continue;
					}
					this.form[i].flag = false;
				}	
			},
		submit() {
				if(this.form.length!==0){
				this.$refs.uForm[0].validate(valid => {
					if (valid) {
					} else {					
					}
					this.form.push({
					startTime: '',
					stopTime: '',
					train: '',
					certificates:"",
					jobTitle:"",	
						flag:true	,
						}
				)
					this.loop();
				});
				}else{
					this.form.push(
	{
		      startTime: '',
					stopTime: '',
					train: '',
					certificates:"",
					jobTitle:"",	
						flag:true	
	});
					this.loop();
				}
			},	
			
			loop() {
				for (var i = 0; i < this.form.length; i++) {
					this.form[i].flag = false;							
				}
				this.form[this.form.length - 1].flag = true;
			},
									
								nextStep(){
									this.bencun(this.form);
									this.can["trainingInformation"]=this.form;
									this.askHou(this.can);
									// this.$emit('nextStep');
								}
		},
			onReady() {
				this.$refs.uForm[0].setRules(this.rules);
			},
			
			mounted(){
				this.$refs.uForm[0].setRules(this.rules);
			}
		
	};
</script>

<style lang="scss" scoped>
	h3{
		font-size: 17px;
		padding: 15px 0;
		font-weight: 560;
		border-bottom: 1px solid #e4e7ed;
		a{
			color: #499CF2;
			float:right;
			text-decoration: none;
			font-weight: normal;
		}
		
		a:nth-child(2){
			margin-right: 30px;
		}
		
	}
	

	
page{
	padding: 0px 15px;
	background-color: #FAFAFA;
}

.icon-xinghao{
color: red;
	}
	
	.add{
		margin-top: 30px!important;
		width: 100%;
		height: 40px;
		text-align: center;
		margin: auto;
		line-height: 40px;
		font-size: 17px;
		border: 1px dashed grey ;
		font-family:"iconfont";
		font-size: 20px;
	}
	
	.next{
    margin-top: 50px!important;
		background-color: #474747;
		height: 50px;
		color: white;
	}
	.bigest{
		// border: 10px solid blue;
	}

</style>
