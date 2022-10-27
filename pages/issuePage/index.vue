<template>
	<view class="apply-panel">
		<uni-forms ref="form" :model="formData" :rules="rules">
			<label class="input-name">企业税号码</label>
			<uni-forms-item  name="Num">
					<uni-easyinput  v-model="formData.Num" placeholder="请输入企业税号码" name="Num"/>
				</uni-forms-item>
		</uni-forms>
		<button class="confirm-btn" @click="confirm">确认</button>
	</view>
</template>

<script>
	export default {
		data(){
			return {
				formData:{
					Num:'',
				},
				rules: {
					Num: {
						rules: [
							{
								required: true,
								errorMessage: '请输入企业税号码',
							},
							{
								minLength: 3,
								maxLength: 5,
								// pattern:/^[A-Z0-9]{15}$|^[A-Z0-9]{17}$|^[A-Z0-9]{18}$|^[A-Z0-9]{20}$/,
								errorMessage: '请输入正确的税号',
							}
						]
					},
				}
			}

		},
		methods:{
			//确认
			confirm(){
				this.$refs.form.validate().then(res=>{
					uni.navigateTo({
						url: '/pages/issueDetail/index',
						
					})

				}).catch(err =>{
					console.log('表单错误信息：', err);
				})
			}
		}
	}
	
</script>

<style lang="scss" scoped>
	.apply-panel{
		width: 95vw;
		margin: 35rpx auto;
		.input-name{
			display: inline-block;
			margin: 10rpx;
			
		}
		.confirm-btn{
			color: #ffffff;
			background: #506AF0;
			line-height: 120rpx;
		}
	}

</style>