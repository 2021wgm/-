<!-- 基本信息页面 -->
<template>
	<view class="content">
		<view class="title">
			基本信息
		</view>
		<u-form :model="basicInformation" ref="uForm" label-width="230" :label-style="label_style" label-position="left" class="form">
			<u-form-item  label="姓名"  prop="name" required>
				<u-input v-model="basicInformation.name" placeholder="请输入" />
			</u-form-item>
			<u-form-item  label="曾用名"  prop="formerName" >
				<u-input v-model="basicInformation.formerName" placeholder="请输入"/>
			</u-form-item>
			<u-form-item  label="姓名全拼"  prop="fullNameSpelling" required>
				<u-input v-model="basicInformation.fullNameSpelling" placeholder="请输入"/>
			</u-form-item>
			<u-form-item  label="英文名称"  prop="englishName" >
				<u-input v-model="basicInformation.englishName" placeholder="请输入"/>
			</u-form-item>
			<u-form-item  label="国籍"  prop="countryOfCitizenship" required>
				<u-input v-model="basicInformation.countryOfCitizenship" placeholder="请输入"/>
			</u-form-item>
			<u-form-item  label="民族"  prop="clan" required>
				<u-input v-model="basicInformation.clan" placeholder="请输入"/>
			</u-form-item>
			<u-form-item  label="证件类型"  prop="typeOfCertificate" required >
				<u-input v-model="basicInformation.typeOfCertificate" type="select" placeholder="请选择" @click="show[0].show = true"/>
				<u-select v-model="show[0].show"  mode="single-column" :list="list1" @confirm="confirm" ></u-select>
			</u-form-item>
			<u-form-item  label="证件号码"  prop="id_number" required>
				<u-input v-model="basicInformation.id_number" placeholder="请输入" @blur='confirm4' @confirm='confirm4'/>
			</u-form-item>
			<u-form-item  label="性别"  prop="sex" required >
				<u-input v-model="basicInformation.sex" type="select" placeholder="请选择" @click="show[1].show = true"/>
				<u-select v-model="show[1].show"  mode="single-column" :list="list2" @confirm="confirm" ></u-select>
			</u-form-item>
			<u-form-item  label="出生日期"  prop="dateOfBirth" required >
				<u-input v-model="basicInformation.dateOfBirth" type="select" placeholder="请选择" @click="show[2].show = true"/>
				<u-calendar v-model="show[2].show" mode="date" @change="confirm1"></u-calendar>
			</u-form-item>
			<u-form-item  label="婚姻状况"  prop="maritalStatus" required >
				<u-input v-model="basicInformation.maritalStatus" type="select" placeholder="请选择" @click="show[3].show = true"/>
				<u-select v-model="show[3].show"  mode="single-column" :list="list3" @confirm="confirm" ></u-select>
			</u-form-item>
			<u-form-item  label="户口性质"  prop="accountType" required >
				<u-input v-model="basicInformation.accountType" type="select" placeholder="请选择" @click="show[4].show = true"/>
				<u-select v-model="show[4].show"  mode="single-column" :list="list4" @confirm="confirm" ></u-select>
			</u-form-item>
			<u-form-item  label="贯籍省份"  prop="hometown" required >
				<u-input v-model="basicInformation.hometown" type="select" placeholder="请选择" @click="show[5].show = true"/>
				<u-picker mode="region" v-model="show[5].show"  @confirm="confirm2"></u-picker>
			</u-form-item>
			<u-form-item  label="特长爱好"  prop="specialHobby" >
				<u-input v-model="basicInformation.specialHobby" placeholder="请输入"/>
			</u-form-item>
			<u-form-item  label="是否应届生入职"  prop="graduates" >
				<u-input v-model="basicInformation.graduates" type="select" placeholder="请选择" @click="show[6].show = true"/>
				<u-select v-model="show[6].show"  mode="single-column" :list="list5" @confirm="confirm" ></u-select>
			</u-form-item>
			<u-form-item  label="手机号码"  prop="phone" required>
				<u-input v-model="basicInformation.phone" placeholder="请输入"/>
			</u-form-item>
			<u-form-item  label="私人邮箱"  prop="email" required>
				<u-input v-model="basicInformation.email" placeholder="请输入"/>
			</u-form-item>
			<u-form-item  label="紧急联系人"  prop="emergencyContact" required>
				<u-input v-model="basicInformation.emergencyContact" placeholder="请输入"/>
			</u-form-item>
			<u-form-item  label="紧急人联系方式"  prop="contactInformation" required>
				<u-input v-model="basicInformation.contactInformation" placeholder="请输入"/>
			</u-form-item>
		</u-form>
		<u-button class="btn" @click="submit">下一步</u-button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				label_style: { fontSize: "30rpx",fontWeight: 'bold'},
				list1:[
					{
						value: '1',
						label: '身份证',
						extra: 'typeOfCertificate'
					},
					{
						value: '2',
						label: '居住证',
						extra: 'typeOfCertificate'
					},
				],
				list2:[
					{
						value: '1',
						label: '男',
						extra: 'sex'
					},
					{
						value: '2',
						label: '女',
						extra: 'sex'
					},
				],
				list3:[
					{
						value: '1',
						label: '已婚',
						extra: 'maritalStatus'
					},
					{
						value: '2',
						label: '未婚',
						extra: 'maritalStatus'
					},
				],
				list4:[
					{
						value: '1',
						label: '农村户口',
						extra: 'accountType'
					},
					{
						value: '2',
						label: '城镇户口',
						extra: 'accountType'
					},
				],
				list5:[
					{
						value: '1',
						label: '是',
						extra: 'graduates'
					},
					{
						value: '2',
						label: '否',
						extra: 'graduates'
					},
				],
				show: [{show:false},{show:false},{show:false},{show:false},{show:false},{show:false},{show:false}],
				basicInformation: {
					id:'',
					name: '',
					formerName: '',
					fullNameSpelling: '',
					englishName: '',
					countryOfCitizenship : '',
					clan : '',
				  typeOfCertificate: '',
					id_number : '',
					sex: '',
					dateOfBirth: '',
					maritalStatus: '',
					accountType : '',
					hometown: '',
					specialHobby: '',
					graduates: '',
					phone: '',
					email: '', 
					emergencyContact: '',
					contactInformation: '',
				},
				rules: {
					name: [
						{
							required: true,
							message: '请输入姓名',
							// 可以单个或者同时写两个触发验证方式
							trigger: 'blur',
						},
					  // 2到10个字符之间的判断
						{
							min: 2,
							max: 10,
							message: '姓名长度在2-10个字符之间',
							trigger: 'blur',
						},
					],
					fullNameSpelling: [
						{
							required: true,
							message: '请输入全拼姓名', 
							// 可以单个或者同时写两个触发验证方式
							trigger: 'blur',
						},
					  // 2到10个字符之间的判断
						{
							pattern: /^[a-zA-Z]*$/g,
							// 正则检验前先将值转为字符串
							transform(value) {
								return String(value);
							},
							message: '只能包含字母',
							trigger: 'blur,change',
						},
					],
					countryOfCitizenship:{
						required: true,
						message: '请输入国籍',
						trigger: 'blur'
					},
					clan:{
						required: true,
						message: '请输入民族',
						trigger: 'blur'
					},
					typeOfCertificate:{
						required: true,
						message: '请选择证件类型',
						trigger: 'blur,change'
					},
					id_number:{
						required: true,
						message: '请输入证件号码',
						trigger: 'change'
					},
					sex:{
						required: true,
						message: '请选择性别',
						trigger: 'blur,change'
					},
					dateOfBirth:{
						required: true,
						message: '请选择出生日期',
						trigger: 'blur,change'
					},
					maritalStatus:{
						required: true,
						message: '请选择婚姻状况',
						trigger: 'blur,change'
					},
					accountType:{
						required: true,
						message: '请选择户口性质',
						trigger: 'blur,change'
					},
					hometown:{
						required: true,
						message: '请选择贯籍省份',
						trigger: 'blur,change'
					},
					phone:[
						{
							required: true,
							message: '请输入手机号码',
							trigger: 'blur'
						},
						{
							// 自定义验证函数，见上说明
							validator: (rule, value, callback) => {
								// this.$u.test.mobile()就是返回true或者false的
								return this.$u.test.mobile(value);
							},
							message: '手机号码不正确',
							// 触发器可以同时用blur和change
							trigger: ['change','blur'],
						}
					],
					contactInformation:[
						{
							required: true,
							message: '请输入紧急联系人联系方式',
							trigger: 'blur'
						},
						{
							// 自定义验证函数，见上说明
							validator: (rule, value, callback) => {
								return this.$u.test.mobile(value);
							},
							message: '手机号码不正确',
							// 触发器可以同时用blur和change
							trigger: ['change','blur'],
						}
					],
					email:[
						{
							required: true,
							message: '请输入私人邮箱',
							// 可以单个或者同时写两个触发验证方式
							trigger: 'blur'
						},
						{
							// 自定义验证函数，见上说明
							validator: (rule, value, callback) => {
								return this.$u.test.email(value);
							},
							message: '邮箱格式不正确',
							// 触发器可以同时用blur和change
							trigger: ['change','blur'],
						},
					],
					emergencyContact:{
						required: true,
						message: '请输入紧急联系人',
						// 可以单个或者同时写两个触发验证方式
						trigger: 'blur'
					},
				}
			}	
		},
		mounted() {
			this.$refs.uForm.setRules(this.rules);
			uni.getStorage({
				key: 'basicInformation',
				success: res=> {
					this.basicInformation = res.data
				},
			}),
			uni.request({
			    url: 'http://192.168.8.120:10001/onboarding/basicinformation/savePerfectPersonalInformation', 
			    data: {
			        basicInformation: []
			    },
					method: 'POST'	,	
			    success: (res) => {
			        // console.log(res.data);
			        this.basicInformation = res.data;
			    }
			});
		},
		methods: {
			submit() {
				this.$refs.uForm.validate(valid => {
					if (valid) {
						// console.log('验证通过');
						uni.setStorage({
							key: 'basicInformation',
							data: this.basicInformation,
						});
						this.$emit('nextStep')
					} else {
						// console.log('验证失败');
					}
				});
			}, 
			confirm(res) {
				this.basicInformation[res[0].extra] = res[0].label
			},
			confirm1(res){
				this.basicInformation.dateOfBirth = res.result
			},
			confirm2(res){
				this.basicInformation.hometown = res.province.label+'-'+res.city.label+'-'+res.area.label
			},
			confirm4(res){
				if(res.length==18){
					if(res[16]%2==1){
						this.basicInformation.sex = '男'
					}else{
						this.basicInformation.sex = '女'
					}
					this.basicInformation.dateOfBirth = res.slice(6,10)+"-"+res.slice(10,12)+"-"+res.slice(12,14)
				}else{
					this.basicInformation.sex = ''
					this.basicInformation.dateOfBirth = ''
				}
			}
		},
		
		onBackPress(){
			return true
		}
	}
</script>

<style scoped>
	.content{
	 display: flex;
	 flex-direction:column;
	}
	.title{
		font-size: 34rpx;
		padding: 30rpx 0;
		font-weight: 560;
		border-bottom: 2rpx solid #e4e7ed;
	}
	.form{
		margin-left: 20rpx;
		margin-right: 20rpx;
	}
	.btn{
		width: 96%;
		margin-top: 100rpx;
		margin-bottom: 100rpx;
		right: 10rpx;
		color: white;
		background: #303133;
		border-radius: 10rpx;
	}

</style>
