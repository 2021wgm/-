<template>
	<view class="bigest">
		
	<div class="box1"  v-for="(item,index) in form">
		
		<h3>职称信息<text v-text="form.length>1?index+1:'' "> </text>
		
	  <a  style="margin-left: 15px;"   v-show="form.length>1" href="javaScript:;"	class="b" @click="zhankai(index)" v-text="item.flag?'收起':'展开' "></a>		
		<a v-show="form.length>1" href="javaScript:;" class="r"   @click="remove(index)">删除</a>
		</h3>
		
		<u-form :model="item" ref="uForm" label-align="left" label-width="80px" v-show="item.flag" :label-style="label_style">
			
			<u-form-item required label="开始时间"  prop="startTime" >
				<u-input v-model="item.startTime"  @click="show1=!show1" placeholder="请输入"  type="select"  />
				<u-calendar v-model="show1" mode="date" @change="change1($event,index)"></u-calendar>
				</u-form-item>
				
      <u-form-item required label="结束时间"  prop="stopTime"  >
				<u-input v-model="item.stopTime" @click="show2=!show2"   placeholder="请输入" type="select" />
		  <u-calendar v-model="show2" mode="date" @change="change2($event,index)"></u-calendar>		
			</u-form-item>
    
			<u-form-item required label="获得职称"  prop="jobTitle" >
			<u-input v-model="item.jobTitle"  placeholder="请输入" />
     </u-form-item>
			
	
			
      <u-form-item required  label="职称等级"   prop="jobLevel"  >
				<u-input v-model="item.jobLevel" placeholder="请输入" />
				</u-form-item>
      
			<u-form-item required label="职称编码"  prop="jobCode" >
						<u-input v-model="item.jobCode"  placeholder="请输入" />
			</u-form-item>
			
			<u-form-item required label="是否最高资格等级"   prop="qualificationLevel" label-width="125px">
							<u-input v-model="item.qualificationLevel"  @click="show =!show" placeholder="请输入"  type="select" />
			        <u-select v-model="show" :list="list" @confirm="confirm($event,index)"></u-select>
			</u-form-item>
     
	</u-form>
	</div>
	
      <div  class="add" @click="submit">+ 新增一条教育信息</div>
			<u-button class="next" @click="nextStep">下一步</u-button>
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
				show1:false,
				show2:false,
								list: [
									{
										value: '1',
										label: '是'
									},
									{
										value: '2',
										label: '否'
									}
								],
			   listObject:[],
				index:1,
				newObj:null,
				show:false,
				form: [{
					startTime: '',
					stopTime: '',
					jobTitle: '',
					jobLevel:"",
					jobCode:"",
					qualificationLevel:"",
					flag:true
				}],
				
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
								jobTitle:[{
									required:true,
									message:"请输入获得的职称",
									trigger: ['change','blur'],						
								}],
								jobLevel:[{
									required:true,
									message:"请输入职称等级",
									trigger: ['change','blur'],						
								}],
								jobCode:[{		
									required:true,
									message:"请输入职称编码",
									trigger: ['change','blur'],						
								}],
								jobCode:[{
									required:true,
									message:"是否是最高资格等级",
									trigger: ['change','blur'],						
								}
								]
				}			
			};
		},
		mounted(){
					this.$refs.uForm[0].setRules(this.rules);
		},
		methods:{
			askHou(can){
				uni.request({
					url:"http://192.168.8.120:10001/onboarding/basicinformation/savePerfectPersonalInformation",
					method:"POST",
					data: can	,	
					success:function(res){
						console.log("向后台发送职称数据",res);
					}
				})
			},	
			bencun(can1){
										 uni.setStorage({
											 key:"positional-info",
											 data:can1
										 })
			},
			getben(){
										 uni.getStorage({
											 key:"positional-info",
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
				console.log("删除项",this.form.splice(index,1));
				if(this.form.length==1){
					this.form[0].flag=true;
				}
			},
				confirm(e,index) {
				this.form[index].qualificationLevel=e[0].label;			
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
					this.form.push(
					{
						startTime: '',
						stopTime: '',
						jobTitle: '',
						jobLevel:"",
						jobCode:"",
						qualificationLevel:"",
						flag:true
					}
				)
					this.loop();
				});
				}else{
					this.form.push(
{
					startTime: '',
					stopTime: '',
					jobTitle: '',
					jobLevel:"",
					jobCode:"",
					qualificationLevel:"",
					flag:true
				}
	);
					this.loop();
				}
			},	
			
			loop() {
				for (var i = 0; i < this.form.length; i++) {
					this.form[i].flag = false;							
				}
				this.form[this.form.length - 1].flag = true;
			},
					
					// formSave:function(){
					// 	  var obj={};
					// 		obj["box1"]=document.querySelector(variable);
					// 		obj["index"]=this.index;
					// 	this.listObject.push(obj);
					// 	this.index++;
					// 	 this.listObject.push(this.newObj);			
					// },
					
					// formAround:function(){
					// 	for(var i=0;i<this.listObject.length;i++){
					// 		// document.querySelector(".bigest").appendChild("");
					// 		console.log(this.listObject);
					// 	}
					// },
								nextStep(){
									this.bencun(this.form);
									this.can["jobInformation"]=this.form;
									this.askHou(this.can);
									
									// this.$emit('nextStep')
								}
			
		},
					
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
