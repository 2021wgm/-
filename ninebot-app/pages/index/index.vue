<template>
	<view class="navbar">
		<NavBar :info="info" :flag="flag" @checkBar="checkBar"></NavBar>
		<BasicInfo v-show="flag===0" @nextStep="nextStep"></BasicInfo>
		<FamilyInfo v-show="flag===1" @nextStep="nextStep"></FamilyInfo>
		<EducationInfo v-show="flag===2" @nextStep="nextStep"></EducationInfo>
		<JobInfo v-show="flag===3" @nextStep="nextStep"></JobInfo>
		<TrainingInfo v-show="flag===4" @nextStep="nextStep"></TrainingInfo>
		<QualificationInfo v-show="flag===5" @nextStep="nextStep"></QualificationInfo>
		<PositionalInfo v-show="flag===6" @nextStep="nextStep"></PositionalInfo>
		<CredentialsInfo v-show="flag===7" @nextStep="nextStep"></CredentialsInfo>
	</view>
	
</template>

<script>
	import NavBar from '../../component/navbar/index.vue'
	import BasicInfo from '../../pages/basic-info/index.vue'
	import CredentialsInfo from '../../pages/credentials-info/index.vue'
	import EducationInfo from '../../pages/education-info/index.vue'
	import FamilyInfo from '../../pages/family-info/index.vue'
	import JobInfo from '../../pages/job-info/index.vue'
	import PositionalInfo from '../../pages/positional-info/index.vue'
	import QualificationInfo from '../../pages/qualification-info/index.vue'
	import TrainingInfo from '../../pages/training-info/index.vue'
	
	export default {
		data() {
			return {
				info:[
					{ 
						id:0,
						active: true,
						text: '基本信息',
						url: '../../pages/basic-info/index'
					},
					{
						id:1,
						active: true,
						text: '家庭信息',
						url: '../../pages/family-info/index'
					},
					{
						id:2,
						active: true,
						text: '教育信息',
						url: '../../pages/education-info/index'
					},
					{
						id:3,
						active: true,
						text: '工作信息',
						url: '../../pages/job-info/index'
					},
					{ 
						id:4,
						active: true,
						text: '培训信息',
						url: '../../pages/training-info/index'
					},
					{
						id:5,
						active: true,
						text: '职业资格信息',
						url: '../../pages/qualification-info/index'
					},
					{
						id:6,
						active: true,
						text: '职称信息',
						url: '../../pages/positional-info/index'
					},
					{
						id:7,
						active: true,
						text: '证件信息上传',
						url: '../../pages/credential-info/index'
					},
				],
				flag: 0,
			}
		},
		components:{ NavBar,BasicInfo,FamilyInfo,EducationInfo,
								 JobInfo,TrainingInfo,QualificationInfo,
								 PositionalInfo,CredentialsInfo },
		methods: {
			nextStep(){
				this.info[this.flag].active = true
				this.flag++
				this.info[this.flag].active = false
			},
			checkBar(id){
				this.flag = id
			}
		},
	}
</script>

<style scoped>
	.navbar{
		border-top: 1px solid #C8C7CC;
		margin-left: 20rpx;
	}
</style>
