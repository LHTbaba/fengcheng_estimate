<template>
	<view class="user-panel">
		<view class="info-panel" @click="logins">
			<image class="avatar" :src="avatar ? avatar : '/static/tabIcon/user.png'"></image>
			<view class="info-text">
				<p class="name">{{name ? name : '微信用户'}}</p>
				<!-- <p class="number">15179413293</p> -->
			</view>
		</view>
		<!-- <button open-type="getPhoneNumber" @getphonenumber="getPhoneNumber" withCredentials="true" @click="getNumber">手机号一键登录</button> -->
		<view class="block-panel">
			<view class="block-item" @click="goComplainList('')">
				<p class="number one-number">{{statisticsNum.QB ? statisticsNum.QB : 0}}</p>
				<p class="title-text">全部问题</p>
			</view>
			<view class="block-item" @click="goComplainList('002')">
				<p class="number two-number">{{statisticsNum.BLZ ? statisticsNum.BLZ : 0}}</p>
				<p class="title-text">办理中</p>
			</view>
		</view>
		<view class="block-panel">
			<view class="block-item" @click="goComplainList('003')">
				<p class="number three-number">{{statisticsNum.YBL ? statisticsNum.YBL : 0}}</p>
				<p class="title-text">已办理</p>
			</view>
			<view class="block-item" @click="goComplainList('004')">
				<p class="number four-number">{{statisticsNum.YBJ ? statisticsNum.YBJ : 0}}</p>
				<p class="title-text">已办结</p>
			</view>
		</view>
		<view class="setting">
			<view class="setting-panel">
				<p class="title">设置选项</p>
				<view class="func-item" @click="logOut">
					<view class="left-content">
						<img src="/static/log-out.png">
						<p class="func-text">退出登录</p>
					</view>
					<view class="right-content">〉</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				avatar: '',
				name: '',
				statisticsNum: {}
			}
		},
		onShow() {
			if(this.$store.state.hasLogin) {
				this.avatar = this.$store.state.userInfo.avatarUrl
				this.name = this.$store.state.userInfo.nickName
				this.getStatistics()
			}
		},
		methods: {
			getNumber() {
				console.log('获取手机号')
			},
			getPhoneNumber(e) {
				console.log(e)
				//e.detail这里会有三个属性
				//encryptedData
				//errMsg
				//iv
			},
      logins() {
				if(this.$store.state.hasLogin) {
					uni.showToast({
						icon: "none",
						title: '已经是登录状态，不用重复登录！'
					})
				}else {
					// 获取用户信息
					uni.getUserProfile({
						desc:"用于完善用户信息",
						success: (res) => {
							this.$store.commit('getUserinfo', res.userInfo)
							this.avatar = res.userInfo.avatarUrl
							this.name = res.userInfo.nickName
							uni.showToast({
								icon:"none",
								title:'获取成功'
							})
							// 获取用户code
							uni.login({
								success: (res) => {
									var code = res.code
									this.$api.login({
										code: code
									}).then(resp => {
										if(resp.openid) {
											this.$store.commit('login', resp)
										}else {
											uni.showToast({
												icon:"none",
												title:'获取openid失败！'
											})
										}
										this.getStatistics()
									}).catch(err => {
										console.log(err)
									})
								},
								fail: (error) => {
									console.log('login failed ' + error)
								}
							})
						},
						fail: (err) => {
							console.log(err)
							uni.showToast({
								icon:"none",
								title:'用户拒绝获取'
							})
						}  
					})
				}
      },
			getStatistics() {
				//获取统计数据
				this.$api.getData({
					'cmd.sqlKey': 'MSP_WX_LZY.SQTJ_LIST',
					'cmd.sqlType': 'proc',
					'cmd.QOPENID': this.$store.state.openid
				}).then(res => {
					this.statisticsNum = res[0]
				}).catch(err => {
					console.log(err)
				})
			},
			logOut() {
				if(this.$store.state.hasLogin) {
					// 登录状态下可退出登录
					uni.showModal({
						title: '提示',
						content: '确认要退出登录吗？',
						success: (res) => {
							if (res.confirm) {
								// 刷新数据
								this.avatar = ''
								this.name = ''
								this.statisticsNum = {}
								uni.showToast({
									icon: "none",
									title: '已退出！'
								})
								this.$store.commit('logout', res)
							}else if (res.cancel) {
								uni.showToast({
									icon: "none",
									title: '用户点击取消'
								})
							}
						}
					})
				}else {
					// 提示用户还未登录
					uni.showToast({
						icon: "none",
						title: '您还未登录呢！'
					})
				}
			},
			goComplainList(status) {
				uni.navigateTo({
					url: `/pages/complainList/index?status=${status}`
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
.user-panel {
	height: 100vh;
	background: #F9FAFB;
	.info-panel {
		height: 184rpx;
		display: flex;
		align-items: center;
		background: #ffffff;
		.avatar {
			width: 130rpx;
			height: 130rpx;
			margin-left: 32rpx;
			border-radius: 50%;
		}
		.info-text {
			margin-left: 32rpx;
			.name {
				color: #202236;
				font-family: Source Han Sans CN;
				font-weight: bold;
				font-size: 36rpx;
				line-height: 48rpx;
			}
			.number {
				color: #595C68;
				font-family: Space Grotesk;
				font-weight: regular;
				font-size: 28rpx;
				line-height: 40rpx;
			}
		}
	}
	.block-panel {
		display: flex;
		justify-content: space-between;
		padding: 32rpx 32rpx 0 32rpx;
		.block-item {
			width: 290rpx;
			height: 164rpx;
			padding-left: 40rpx;
			display: flex;
			flex-direction: column;
			justify-content: center;
			border-radius: 20rpx;
			background: #ffffff;
			.number {
				font-family: Poppins;
				font-weight: SemiBold;
				font-size: 40rpx;
				line-height: 48rpx;
			}
			.one-number {
				color: #B45309;
			}
			.two-number {
				color: #4178D4;
			}
			.three-number {
				color: #52B6DF;
			}
			.four-number {
				color: #F59E0B;
			}
			.title-text {
				color: #64748B;
				font-family: Poppins;
				font-weight: medium;
				font-size: 26rpx;
				line-height: 44rpx;
			}
		}
	}
	.setting {
		padding: 32rpx;
		.setting-panel {
			padding: 32rpx;
			border-radius: 20rpx;
			background: #ffffff;
		}
		.title {
			color: #BEBFC2;
			font-family: Source Han Sans CN;
			font-weight: regular;
			font-size: 24rpx;
			line-height: 40rpx;
		}
		.func-item {
			height: 128rpx;
			padding: 0 12rpx;
			display: flex;
			align-items: center;
			justify-content: space-between;
			border-bottom: 2rpx solid #F1F2F3;
			.left-content {
				display: flex;
				img {
					width: 40rpx;
					height: 40rpx;
				}
				.func-text {
					margin-left: 46rpx;
					color: #202236;
					font-family: Source Han Sans CN;
					font-weight: medium;
					font-size: 28rpx;
				}
			}
			.right-content {
				color: #BEBFC2;
			}
		}
	}
}
</style>
