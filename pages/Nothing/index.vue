<template>
	<view>
		<view class="problem-item" v-for="(item,index) in qidList" :key="index" >
			<view class="problem-title">
				<span>{{ index + 1 }}.</span>{{item.QName}}
				<span v-if="item.childs[0].QUESTION_TYPE == 1">（单选）</span>
				<span v-if="item.childs[0].QUESTION_TYPE == 2">（多选）</span>
			</view>
			<view class="select-panel select-panel-last" v-if="item.childs[0].QUESTION_TYPE == 1">
				<radio-group  >
					<label v-for="(items,i) in item.childs" :key="i">
						<view>
							<radio :value="items.OPTION_VALUE" checked="items.SFYX == 1" disabled="item.SFYX == 0"/>
						</view>
						<view>{{items.OPTION_VALUE}}.{{items.OPTION_NAME}}</view>
					</label>
				</radio-group>
			</view>
			<view class="select-panel select-panel-last" v-if="item.childs[0].QUESTION_TYPE == 2">
				<checkbox-group @change="checkboxChange($event, item)" >
					<label v-for="(items,i) in item.childs" :key="i">
						<view >
							<checkbox :value="items.OPTION_VALUE" v-if="items.SFYX == 1 " checked disabled/>
							<checkbox :value="items.OPTION_VALUE" v-else disabled/>
						</view>
						<view>{{items.OPTION_VALUE}}.{{items.OPTION_NAME}}</view>
						
					</label>
				</checkbox-group>
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		data() {
			return {
				mapParam: '',
				qidList: [],
				resArray: [],
			}
		},
		onLoad(data) {
			// console.log(data)
			const temp = JSON.parse(data.data)
			this.mapParam = temp.mapParam
			const qidList = []
			temp.LIST.forEach(item => {
				const parent = qidList.find(e => e.QID === item.QID)
				if (parent) {
					parent.childs.push(item)

				} else {
					const list = {
						QID: item.QID,
						QName: item.QUESTION_NAME,
						childs: [item]
					}
					qidList.push(list)
				}
			})
			this.qidList = qidList;
			console.log(this.qidList)
		},
		methods :{
			checkboxChange(e,item){
				console.log(item)
			}
		}
	}
</script>

<style lang="scss" scoped>
	.problem-item {
		padding: 48rpx;
	
		.red {
			color: #FF8A8A;
		}
	
		.problem-title {
			margin-bottom: 48rpx;
			color: #000000;
			font-family: Source Han Sans CN;
			font-weight: medium;
			font-size: 32rpx;
		}
	
		.sub-title {
			padding-left: 48rpx;
			color: #000000;
			font-family: Source Han Sans CN;
			font-weight: medium;
			font-size: 28rpx;
			line-height: 74rpx;
	
			.gray {
				color: #BFBFBF;
			}
		}
	
		.select-panel {
			padding-left: 48rpx;
	
			input {
				height: 73.6rpx;
				padding: 0 10rpx;
				border-radius: 4rpx;
				background: #FFFFFF;
				border: 2rpx solid #E0E6E4;
				font-size: 28rpx;
			}
	
			.input {
				height: 73.6rpx;
				padding: 0 10rpx;
				border-radius: 4rpx;
				background: #FFFFFF;
				color: #868484;
				border: 2rpx solid #E0E6E4;
				font-size: 28rpx;
				line-height: 73.6rpx;
			}
	
			label {
				display: flex;
				line-height: 100rpx;
			}
		}
	
		.ks {
			margin-top: 20rpx;
		}
	
		.select-panel-last {
			margin-bottom: 40rpx;
		}
	}
</style>