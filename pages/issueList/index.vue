<template>
	<view>
		<view class="problem-item" v-for="(item,index) in qidList" :key="index">
			<view class="problem-title">
				<span>{{ index + 1 }}.</span>{{item.QName}}
				<span v-if="item.childs[0].QUESTION_TYPE == 1">（单选）</span>
				<span v-if="item.childs[0].QUESTION_TYPE == 2">（多选）</span>
			</view>
			<view class="select-panel select-panel-last" v-if="item.childs[0].QUESTION_TYPE == 1">
				<radio-group @change="onChangeCheckVal($event, item.QID)" v-model="ruleForm.resource[index]">
					<label v-for="(items,i) in item.childs" :key="i">
						<view>
							<radio :value="items.OPTION_VALUE" :checked="item.checked" />
						</view>
						<view>{{items.OPTION_VALUE}}.{{items.OPTION_NAME}}</view>
					</label>
				</radio-group>
			</view>
			<view class="select-panel select-panel-last" v-if="item.childs[0].QUESTION_TYPE == 2">
				<checkbox-group @change="onChangeCheckVal($event, item.QID)" v-model="ruleForm.resource[index]">
					<label v-for="(items,i) in item.childs" :key="i">
						<view>
							<checkbox :value="items.OPTION_VALUE" :checked="index === current" />
						</view>
						<view>{{items.OPTION_VALUE}}.{{items.OPTION_NAME}}</view>
					</label>
				</checkbox-group>
			</view>

		</view>
		<view class="submit-panel">
			<button @click="submitForm(item)">提交</button>
		</view>
	</view>

</template>

<script>
	export default {
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
		},
		methods: {
			submitForm() {
				
				if (this.resArray.length !== this.qidList.length) {
					
					uni.showToast({
						title: '请填写所有题目！',
						icon: 'none',
						mask: true,
					})
					return
				}

				let data = {
					'cmd.sqlKey': 'SF_EDU.ADD_DTJL',
					'cmd.sqlType': 'proc',
					'cmd.QOPENID': this.$store.state.openid, //openid
					'cmd.QWJID': this.mapParam.QWJID, //问卷id
					'cmd.QQYSH': this.mapParam.QQYSH, //企业税号
					'array.arr': this.resArray.length, //数组长度	
					// 'res': this.resArray,
					// 'arr1.para1': JSON.stringify(resArrayQid), //题目id
					// 'arr1.para2': JSON.stringify(resArrayVal), //用户选项
				}
				
				this.resArray.forEach((val, index) => {
					let temp = val.split(':')
					data[`arr${index + 1}.para${1}`] = temp[0]
					data[`arr${index + 1}.para${2}`] = temp[1]
					
					if (!temp[0] || !temp[1]) {
						uni.showToast({
							title: '请填写所有题目！',
							icon: 'none',
							mask: true,
						})
						return
					}
					// resArrayQid[index] = temp[0]
					// resArrayVal[index] = temp[1]
				})
				
				console.log(data)
				// return
				uni.request({
					method: 'POST',
					url: 'https://fcmsp.zjwq.net/loadDataNoReturnCA',
					header: { 
						'content-type': 'application/x-www-form-urlencoded',
						},
					data: data,
					success: ((res) => {
						// console.log(res.data)
						uni.showToast({
							title: '提交成功',
							icon: 'none',
							mask: true,
						})
						setTimeout(function() {
							uni.navigateBack({
								delta:1,
							});
						},1000)
						
					
					})
				})
			},
			onChangeCheckVal(e, qid) {
				// console.log(e.detail.value, qid)
				if (this.resArray.map((item) => item.split(':')[0]).includes(qid)) {
					for (let i = 0; i < this.resArray.length; i++) {
						if (this.resArray[i].includes(qid)) {
							this.resArray[i] = `${qid}:${e.detail.value}`;
						}
					}
				} else {
					this.resArray.push(`${qid}:${e.detail.value}`)
				}
				// console.log(this.resArray)
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

	.submit-panel {
		padding: 50rpx 50rpx 100rpx 50rpx;

		button {
			color: #ffffff;
			background: #506AF0;
			line-height: 120rpx;
		}
	}
</style>
