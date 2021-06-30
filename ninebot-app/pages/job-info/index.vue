<template>
	<view class="bigest">
		
	<div class="box1"  v-for="(item,index) in form">
		
		<h3>工作信息<text v-text="form.length>1?index+1:'' "> </text>
			<img :src="item.src" v-show="form.length>1">
		<a  style="margin-right: 15px;" v-show="form.length>1"  href="javaScript:;"	class="b" @click="zhankai(index)" v-text="item.flag?'收起':'展开' "></a>

	
    <a    v-show="form.length>1"   style="margin-right:15px;"  href="javaScript:;" class="r"  @click="remove(index)">删除</a>	
		
		</h3>
		
		<u-form :model="item" ref="uForm" label-align="left" label-width="75px" v-show="item.flag" :label-style="label_style" >
			
			<u-form-item required label="开始时间"  prop="startTime" >
				<u-input v-model="item.startTime"  @click="show1=!show1" placeholder="请输入" type="select" />
				<u-calendar v-model="show1" mode="date" @change="change1($event,index)"></u-calendar>
				</u-form-item>
				
      <u-form-item required label="结束时间"  prop="stopTime"  >
				<u-input v-model="item.stopTime" @click="show2=!show2"   placeholder="请输入"  type="select" />
		  <u-calendar v-model="show2" mode="date" @change="change2($event,index)"></u-calendar>		
			</u-form-item>
    
			<u-form-item required label="公司名称"  prop="company"  >
			<u-input v-model="item.company"  placeholder="请输入"  />		
     </u-form-item>
			
			
      <u-form-item required label="工作部门"   prop="department"  >
				<u-input v-model="item.department" placeholder="请输入" @click="show3=!show3" type="select" />
				<u-select v-model="show3" mode="single-column" :list="list1" @confirm="confirm($event,index)"></u-select>				
				</u-form-item>
      
			<u-form-item required label="岗位名称"   prop="position"  >
				<u-input v-model="item.position"  placeholder="请输入" />
			</u-form-item>
			
      <u-form-item label="下属人数"  prop="number"  >
				<u-input v-model="form.number"   placeholder="请输入"/>
			</u-form-item>
      
			<u-form-item required label="证明人"  prop="witness"  >
				<u-input v-model="item.witness"   placeholder="请输入"  />
			</u-form-item>
			
			<u-form-item required  label="证明人联系方式"  prop="contact" label-width="115px">
				<u-input v-model="item.contact"   placeholder="请输入" />
			</u-form-item>
	</u-form>
	</div>
	
      <div  class="add" @click="submit">+ 新增一条工作信息</div>
			<u-button class="next" @click="nextStep()">下一步</u-button>
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
				show3:false,
				san1:"../../static/zhengsan.png",
				san2:"../../static/daosan.png",
				list1: [
									{
										value: '1',
										label: '财务部'
									},
									{
										value: '2',
										label: '市场部'
									},
									{
										value: '3',
										label: '采购部'
									},
									{
										value: '4',
										label: '技术部'
									},
									{
										value: '4',
										label: '生产部'
									},
									{
										value: '5',
										label: '质检部'
									},
									{
										value: '6',
										label: '工程部'
									},
									{
										value: '7',
										label: '项目部'
									},
									{
										value: '8',
										label: '综合管理部'
									},
								],
				form: [{
					src:"../../static/zhengsan.png",
        startTime:"",
				stopTime:"",
				company:"",
				department:"",
				position:"",
				number:"",
				witness:"",
				contact:"",
				flag:true
				}],
				// show:false,
				// mode: 'date',
				rules:{
        startTime:[{
									  required:true,
									 	message:"请输入开始时间",
									 	trigger: ['change','blur'],	
								 }],
				stopTime:[{
									  required:true,
									 	message:"请输入结束时间",
									 	trigger: ['change','blur'],	
								 }],
				company:[{
									  required:true,
									 	message:"请输入公司名称",
									 	trigger: ['change','blur'],	
								 }],
				department:[{
									  required:true,
									 	message:"请输入工作部门",
									 	trigger: ['change','blur'],	
								 }],
				position:[{
									  required:true,
									 	message:"请输入岗位名称",
									 	trigger: ['change','blur'],	
								 }],
				number:[{
									  // required:true,
									 	// message:"请输入下属人数",
									 	// trigger: ['change','blur'],	
								 }],
				witness:[{
									  required:true,
									 	message:"请输入证明人",
									 	trigger: ['change','blur'],	
								 }],
				contact:[{
									  required:true,
									 	message:"请输入证明人联系方式",
									 	trigger: ['change','blur'],	
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
								 ],							
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
						console.log("向后台发送工作数据",res);
					}
				})
			},	
			bencun(can1){
										 uni.setStorage({
											 key:"education-info",
											 data:can1
										 })
			},
			getben(){
										 uni.getStorage({
											 key:"education-info",
											 success:function(res){
												 this.form=res.data;
												 // console.log(this.form);
											 }
										 })
			},
			
			confirm(e,index) {
				this.form[index].department=e[0].label;
	
			},
			
			submit() {
				if(this.form.length!==0){
				this.$refs.uForm[0].validate(valid => {
					if (valid) {
					} else {					
					}
					this.form.push(
{
	      src:"../../static/zhengsan.png",
        startTime:"",
				stopTime:"",
				company:"",
				department:"",
				position:"",
				number:"",
				witness:"",
				contact:"",
					flag:true
				}
					);
					this.loop();
				});
				}else{
					this.form.push(
	{
		src:"../../static/zhengsan.png",
		startTime: '',
		stopTime:"",
		sname:"",
	 scholar:"",
	 smethod:"",
	 schoolSystem:"",
	  graduatetp:"",
	 highestxl:"",
	 firstxl:"",
	 higestxw:"",
	 schooltp:"",
	 xw:"",
	 flag:true
	}
					);
					this.loop();
				}
			},	
			
			change1(e,index) {
				console.log(e.result);
					this.form[index].startTime=e.result;
				},
				change2(e,index){
							this.form[index].stopTime=e.result;
				},
					
					formSave:function(){
						  var obj={};
							obj["box1"]=document.querySelector(variable);
							obj["index"]=this.index;
						this.listObject.push(obj);
						this.index++;
						 this.listObject.push(this.newObj);			
					},
					//每次将盒子作为一个新对象的box1属性保存起来,并为这个对象添加易于区分不同box1盒子的index属性，存储到listObject数组中
					
					formAround:function(){
						for(var i=0;i<this.listObject.length;i++){
							// document.querySelector(".bigest").appendChild("");
							console.log(this.listObject);
						}
					},
					//遍历渲染出所有的家庭成员信息，
					
					loop() {
						for (var i = 0; i < this.form.length; i++) {
							this.form[i].flag = false;	
												
						}
							this.form[this.form.length - 1].flag = true;
					},
					remove(index){
						this.form.splice(index,1);
										if(this.form.length==1){
											this.form[0].flag=true;
										}
					},
					zhankai(index) {
						if(this.form[index].src=="../../static/zhengsan.png"){
							console.log(1);
							this.form[index].src="../../static/daosan.png";
						}else{
							console.log(2);
							this.form[index].src="../../static/zhengsan.png"
						}
						
						
					    
						for (var i = 0; i < this.form.length; i++) {
							if(i==index){
									this.form[index].flag =!this.form[index].flag ;
								continue;
							}
							this.form[i].flag = false;
						}
					// console.log(this.form)		
					},
						nextStep(){
								// 	this.$emit('nextStep');
								this.bencun(this.form);
							this.can["workInformation"]=this.form;					
								this.askHou(this.can);
								}
		},
			onReady() {
				this.$refs.uForm[0].setRules(this.rules);
			},
			
	};
</script>

<style lang="scss" scoped>
	h3{
		font-size: 17px;
		padding: 15px 0;
		font-weight: 560;
		border-bottom: 1px solid #e4e7ed;
		position: relative;
		a{
			color: #499CF2;
			float:right;
			text-decoration: none;
			font-weight: normal;
		}
		
		a:nth-child(2){
			margin-right: 30px;
		}
		
		img{
    position: absolute;
		width: 10px;
		height: 10px;
    right: -2px;
    top: 21px;
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

 
 
 
 
 
