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
			<view v-for="(item,issueDetail) in issueDetail" >
				<view class="issue-list" @click="goAnswer">
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
		</view>
		<view class="" v-show="Inv == 1">
			<view v-for="(item,index) in novel" :key = "item.id" v-if="item.state==1">
				<view class="issue-list" @click="goAnswer">
					<view class="issue-title">{{item.title}}</view>
					<view class="issue-state">
						<view>
							<icon type="" v-if="item.state==0" class="block-noyet"></icon>
							<icon type="" v-if="item.state==1" class="block-done"></icon>
							<text v-if="item.state==0" class="state-noyet">{{item.state==0?'未填':item.state==1?'已填':''}}</text>
							<text v-else="item.state==1" class="state-done">{{item.state==0?'未填':item.state==1?'已填':''}}</text>
						</view>
						<view class="num">问题数量：{{item.num}}题</view>
					</view>
				</view>
			</view>
		</view>
		<view class="" v-show="Inv == 2">
			<view v-for="(item,index) in novel" :key = "item.id" v-if="item.state==0">
				<view class="issue-list" @click="goAnswer">
					<view class="issue-title">{{item.title}}</view>
					<view class="issue-state">
						<view>
							<icon type="" v-if="item.state==0" class="block-noyet"></icon>
							<icon type="" v-if="item.state==1" class="block-done"></icon>
							<text v-if="item.state==0" class="state-noyet">{{item.state==0?'未填':item.state==1?'已填':''}}</text>
							<text v-else="item.state==1" class="state-done">{{item.state==0?'未填':item.state==1?'已填':''}}</text>
						</view>
						<view class="num">问题数量：{{item.num}}题</view>
					</view>
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
				novel:[
					{
						id:'0',
						title:'针对企业营商环境评价专题调研',
						state:'1',
						num:'20',
					},
					{
						id:'1',
						title:'针对企业营商环境评价专题调研',
						state:'0',
						num:'30',
					},
					{
						id:'2',
						title:'针对企业营商环境评价专题调研',
						state:'0',
						num:'30',
					},
				]
			}
			
		},
		onLoad(data) {
			
			// console.log(JSON.parse(data.list))
			this.issueDetail = JSON.parse(data.list)
			console.log(this.issueDetail)
		},
		methods: {
			changeTab(Inv){
				that.navIdx = Inv;
				console.log(that.navIdx)
			},
			
			goAnswer(){
				uni.navigateTo({
					url: '/pages/issueList/index',	
				})
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