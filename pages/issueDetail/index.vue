<template>
	<view class="apply-page">
		<view class="tabs">
			<view :class="['tabs-names',Inv==0?'inv-h-se':'']" @click="Inv=0">
				全部
			</view>
			<view :class="['tabs-names',Inv==1?'inv-h-se':'']" @click="Inv=1">
				已填
			</view>
			<view :class="['tabs-names',Inv==2?'inv-h-se':'']" @click="Inv=2">
				未填
			</view>
		</view>
		<view class="" v-show="Inv == 0" >
			<view v-for="item in issueDetail">
				<view class="issue-list" @click="goAnswer(item)">
					<view class="issue-title">{{item.WJ_NAME}}</view>
					<view class="issue-state">
						<view>
							<icon type="" v-if="item.SFYT==0" class="block-noyet"></icon>
							<icon type="" v-if="item.SFYT==1" class="block-done"></icon>
							<text v-if="item.SFYT==0" class="state-noyet">{{item.SFYT==0?'未填':item.SFYT==1?'已填':''}}</text>
							<text v-else-if="item.SFYT==1" class="state-done">{{item.SFYT==0?'未填':item.SFYT==1?'已填':''}}</text>
						</view>
						<view class="num">问题数量：{{item.QUESTION_NUM}}题</view>
					</view>
				</view>

			</view>
			<view v-if="issueDetail.length <= 0">
				<view class="issue-none" >
					暂无问卷！
				</view>
			</view>
		</view>
		<view class="" v-show="Inv == 1">
			<view v-for="item in issueDetail"  v-if="item.SFYT==1">
				<view class="issue-list" @click="goAnswer(item)">
					<view class="issue-title">{{item.WJ_NAME}}</view>
					<view class="issue-state">
						<view>
							<icon type="" v-if="item.SFYT==0" class="block-noyet"></icon>
							<icon type="" v-if="item.SFYT==1" class="block-done"></icon>
							<text v-if="item.SFYT==0" class="state-noyet">{{item.SFYT==0?'未填':item.SFYT==1?'已填':''}}</text>
							<text v-else-if="item.SFYT==1" class="state-done">{{item.SFYT==0?'未填':item.SFYT==1?'已填':''}}</text>
						</view>
						<view class="num">问题数量：{{item.QUESTION_NUM}}题</view>
					</view>
				</view>
			</view>
			<view v-if="issueDetail.length <= 0">
				<view class="issue-none" >
					暂无问卷！
				</view>
			</view>
		</view>
		<view class="" v-show="Inv == 2">
			<view v-for="item in issueDetail"  v-if="item.SFYT==0">
				<view class="issue-list" @click="goAnswer(item)">
					<view class="issue-title">{{item.WJ_NAME}}</view>
					<view class="issue-state">
						<view>
							<icon type="" v-if="item.SFYT==0" class="block-noyet"></icon>
							<icon type="" v-if="item.SFYT==1" class="block-done"></icon>
							<text v-if="item.SFYT==0" class="state-noyet">{{item.SFYT==0?'未填':item.SFYT==1?'已填':''}}</text>
							<text v-else-if="item.SFYT==1" class="state-done">{{item.SFYT==0?'未填':item.SFYT==1?'已填':''}}</text>
						</view>
						<view class="num">问题数量：{{item.QUESTION_NUM}}题</view>
					</view>
				</view>
			</view>
			<view v-if="issueDetail.length <= 0">
				<view class="issue-none" >
					暂无问卷！
				</view>
			</view>
		</view>
	</view>
</template>

  

<script>
	export default {

		data(){
			return {
				Inv:0,
				issueDetail:[],
				num:null,
			}
			
		},
		onLoad(data) {
			this.num = JSON.parse(data.num)
		},
		onShow() {
			uni.request({
				method:'POST',
				header: {
					'content-type': 'application/x-www-form-urlencoded',
					},
				url: "https://fcmsp.zjwq.net/loadDataNoReturnCA",
				data: {
					'cmd.sqlKey':'SF_EDU.WJ_LIST',
					'cmd.sqlType':'proc',
					'cmd.QQYSH':this.num,
				},
				success: ((res) => {
					this.issueDetail = res.data[0].LIST
				})
			})
		},
		methods: {
			changeTab(Inv){
				that.navIdx = Inv;
				console.log(that.navIdx)
			},
			
			goAnswer(item){
				if(item.SFYT == 1){
					// uni.navigateTo({
					// 	url: '/pages/Nothing/index',	
					// })
					uni.request({
						method:'GET',
						url:'https://fcmsp.zjwq.net/loadDataNoReturnCA',
						data: {
							'cmd.sqlKey':'SF_EDU.SHOW_FINISH_QUESTIONS',
							'cmd.sqlType':'proc',
							'cmd.QOPENID': this.$store.state.openid,	//openid
							'cmd.QQYSH': item.QYSH,
							'cmd.QWJID': item.WJID,
						},
						success: ((res) => {
							console.log(res.data[0]);
							var issue = res.data;
							uni.navigateTo({
								url: '/pages/Nothing/index?data=' + JSON.stringify(res.data[0]),	
							})
						})
					})
				} else {
					uni.request({
						method:'GET',
						url:'https://fcmsp.zjwq.net/loadDataNoReturnCA',
						data: {
							'cmd.sqlKey':'SF_EDU.SHOW_QUESTIONS',
							'cmd.sqlType':'proc',
							'cmd.QOPENID': this.$store.state.openid,	//openid
							'cmd.QQYSH': item.QYSH,
							'cmd.QWJID': item.WJID,
						},
						success: ((res) => {
							console.log(res.data[0]);
							// var issue = res.data;
							uni.navigateTo({
								url: '/pages/issueList/index?data=' + JSON.stringify(res.data[0]),	
							})
						})
					})
				}
			},
		}
    }

</script>
          

<style lang="scss" scoped>
	.apply-page{
		.tabs{
			background-color: #FFFFFF;
			height: 100upx;
			display: flex;
		}
		.tabs-names{
			font-size: 30upx;
			flex: 1;
			text-align: center;
			color: #C9C9C9;
			height: 100upx;
			line-height: 100upx;
		}
		
		.inv-h-se{
			color: #5BA7FF;
			border-bottom: 4upx solid #5BA7FF;
		}
		.issue-none{
			font-size: 40rpx;
			padding: 40rpx 20rpx;
		}
		.issue-list{
			width: 90vw;
			height: 180rpx;
			background-color: #FFFFFF;
			margin: 20rpx auto;
			padding: 60rpx 0 0 40rpx;
			border-radius: 10rpx;
			.issue-title{
				margin-bottom: 30rpx;
			}
			.issue-state{
				display: flex;
				.state-noyet{
					padding-right: 70rpx;
					color: #F2A83B;
				}
				.state-done{
					padding-right: 70rpx;
					color: #25B76E;
				}
				.num{
					color: #DEDEDE;
				}
			}
		}
	}
	.block-noyet{
		width: 10rpx;
		height: 10rpx;
		display: inline-block;
		background-color: #F2A83B;
		transform: translateY(-5rpx);
		margin-right: 10rpx;
	}
	.block-done{
		width: 10rpx;
		height: 10rpx;
		display: inline-block;
		background-color: #25B76E;
		transform: translateY(-5rpx);
		margin-right: 10rpx;
	}
</style>
<style>
	page{
		background-color: #F6F8FB;
	}
</style>