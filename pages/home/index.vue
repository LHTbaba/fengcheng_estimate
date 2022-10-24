<template>
	<view class="home-panel">
		<view class="title">
			事事
			<span class="blue">宜人-丰城码上评</span>
		</view>
		<view class="apply-panel" @click="goComplain">
			<img src="../../static/icon_01.png" class="icon-panel">
			<view class="text-panel">
				<p class="text-title">企业诉求申请（宜春市级层面）</p>
				<p class="text-content">营商环境诉求申请</p>
			</view>
			<view class="arrow">〉</view>
		</view>
		<view class="declared-panel">
			<p class="text-title">事事宜人丰城“码”上评</p>
			<p class="sub-title">企业家、市民朋友：</p>
			<p class="content-title">您好！</p>
			<p class="text-content">如果您在我市部门单位办事过程中，或者接受相关单位监督检查执法过程中遇到任何问题，请您如实反映，我们将严格为您保密，并对您反映的问题进行调查处理。</p>
			<p class="sub-title right">丰城市营商办</p>
		</view>
		<view class="problem-item">
			<view class="problem-title">
				01 您所要评价或反映问题的单位是？
			</view>
			<view class="sub-title">
				<span class="red">*</span>单位名称<span class="gray">（必填项）</span>
			</view>
			<view class="select-panel">
				<!-- <hpy-form-select :dataList="uniList" text="DEPT_NAME" name="ID" v-model="form.uniValue" @change="changeUnion" /> -->
				<view class="input" @click="openMpv()">{{uniName ? uniName : '请选择'}}</view>
				<mpv ref='mpv' :list="uniList" @changeUnion="changeUnionNew" @search="changeUniList"></mpv>
			</view>
			<view class="sub-title">
				<span class="red">*</span>岗位名称<span class="gray">（必填项）</span>
			</view>
			<view class="select-panel">
				<hpy-form-select :dataList="postList" text="JOB_NAME" name="ID" v-model="form.postValue"/>
			</view>
			<view class="select-panel ks" v-if="form.postValue == '0'">
				<input type="text" v-model="form.postName" placeholder="请输入科室名称或工作人员名称" />
			</view>
			<view class="sub-title">
				您的联系方式<span class="gray">（选填项）</span>
			</view>
			<view class="select-panel select-panel-last">
				<input type="text" v-model="form.number" placeholder="请输入您的联系方式" @input='inputNumber' />
			</view>
			<view class="problem-title">
				02 <span class="red">*</span>该单位（岗位）人员服务态度怎么样？
			</view>
			<view class="select-panel select-panel-last">
				<radio-group @change="radioChangeOne">
					<label v-for="(item, index) in oneList" :key="index">
						<view>
							<radio :value="item.value" :checked="item.value === form.oneCurrent" />
						</view>
						<view>{{item.name}}</view>
					</label>
				</radio-group>
				<view v-if="form.oneCurrent === 3">
					<input v-model="form.oneInput" type="text" placeholder="请输入不满意原因（非必填）" />
				</view>
				<view v-if="form.oneCurrent === 4">
					<input v-model="form.twoInput" type="text" placeholder="请输入其他原因（必填）" />
				</view>
			</view>
			<view class="problem-title">
				03 <span class="red">*</span>该单位（岗位）人员办事效率怎么样？
			</view>
			<view class="select-panel select-panel-last">
				<radio-group @change="radioChangeTwo">
					<label v-for="(item, index) in oneList" :key="index">
						<view>
							<radio :value="item.value" :checked="item.value === form.twoCurrent" />
						</view>
						<view>{{item.name}}</view>
					</label>
				</radio-group>
				<view v-if="form.twoCurrent === 3">
					<input v-model="form.threeInput" type="text" placeholder="请输入不满意原因（非必填）" />
				</view>
				<view v-if="form.twoCurrent === 4">
					<input v-model="form.fourInput" type="text" placeholder="请输入其他原因（必填）" />
				</view>
			</view>
			<view class="problem-title">
				04 <span class="red">*</span>您在办事或受监督检查执法过程中是否碰到过吃拿卡要问题？
			</view>
			<view class="select-panel select-panel-last">
				<radio-group @change="radioChangeThree">
					<label v-for="(item, index) in threeList" :key="index">
						<view>
							<radio :value="item.value" :checked="item.value === form.threeCurrent" />
						</view>
						<view>{{item.name}}</view>
					</label>
				</radio-group>
				<view v-if="form.threeCurrent === 1">
					<input v-model="form.fiveInput" type="text" placeholder="请输入详细情况（必填）" />
				</view>
			</view>
			<view class="problem-title">
				05 <span class="red">*</span>您对该单位的本次服务整体评价怎样？
			</view>
			<view class="select-panel select-panel-last">
				<radio-group @change="radioChangeFour">
					<label v-for="(item, index) in fourList" :key="index">
						<view>
							<radio :value="item.value" :checked="item.value === form.fourCurrent" />
						</view>
						<view>{{item.name}}</view>
					</label>
				</radio-group>
				<view v-if="form.fourCurrent === 3">
					<input v-model="form.sixInput" type="text" placeholder="请输入不满意原因（非必填）" />
				</view>
			</view>
			<view class="problem-title">
				06 您还有其他情况反映吗？
			</view>
			<view class="select-panel">
				<input v-model="form.sevenInput" name="form.sevenInput" type="text" placeholder="请输入反映情况" />
			</view>
		</view>
		<view class="submit-panel">
			<button @click="submitForm">提交</button>
		</view>
		<view class="submit-panel-bottom">
			<button @click="toInput('1')" class="one">企业诉求</button>
			<button @click="toInput('2')" class="two">投诉建议</button>
		</view>
	</view>
</template>

<script>
import mpv from '../../components/my-picker-view.vue'
export default {
	data() {
		return {
			uniName: '',
			form: {
				uniValue: '',
				postValue: '',
				postName: '',
				oneCurrent: '',
				number: '',
				twoCurrent: '',
				threeCurrent: '',
				fourCurrent: '',
				oneInput: '',
				twoInput: '',
				threeInput: '',
				fourInput: '',
				fiveInput: '',
				sixInput: '',
				sevenInput: ''
			},
			uniList: [],
			postList: [],
			oneList: [{
				name: '满意',
				value: 0
			}, {
				name: '基本满意',
				value: 1
			}, {
				name: '一般',
				value: 2
			}, {
				name: '不满意',
				value: 3
			}, {
				name: '其他',
				value: 4
			}],
			threeList: [{
				name: '没碰到',
				value: 0
			}, {
				name: '碰到',
				value: 1
			}],
			fourList: [{
				name: '满意',
				value: 0
			}, {
				name: '基本满意',
				value: 1
			}, {
				name: '一般',
				value: 2
			}, {
				name: '不满意',
				value: 3
			}]
		}
	},
	components: {
		mpv
	},
	onShow() {
		this.getUnion()
	},
	watch: {
		form: {
			handler(val, oldVal) {
				if(val.oneCurrent !== 3) {
					this.form.oneInput = ''
				}
				if(val.oneCurrent !== 4) {
					this.form.twoInput = ''
				}
				if(val.twoCurrent !== 3) {
					this.form.threeInput = ''
				}
				if(val.twoCurrent !== 4) {
					this.form.fourInput = ''
				}
				if(val.threeCurrent !== 1) {
					this.form.fiveInput = ''
				}
				if(val.fourCurrent !== 3) {
					this.form.sixInput = ''
				}
				if(val.postValue !== '0') {
					this.form.postName = ''
				}
			},
			deep: true //true 深度监听
		}
	},
	methods: {
		toInput(type) {
			uni.navigateTo({
				url: `/pages/inputSubmit/index?type=${type}`
			})
		},
		openMpv(){
			this.$refs.mpv.toggle('bottom')
		},
		// 电话号码校验
		inputNumber(e) {
			const o = e.detail.value
			const inputRule = /[^\d]/g
			this.$nextTick(() => {
				this.form.number = o.replace(inputRule, '')
			})
		},
		goComplain() {
			uni.navigateTo({
				url: '/pages/complain/index'
			})
		},
		getUnion() {
			this.$api.getData({
				'cmd.sqlKey': 'MSP_WX_LHC.SEL_SELECT',
				'cmd.sqlType': 'proc'
			}).then(res => {
				this.uniList = res[0].LIST
			}).catch(err => {
				console.log(err)
			})
		},
		changeUniList(e) {
			this.$api.getData({
				'cmd.sqlKey': 'MSP_WX_LHC.SEL_SELECT',
				'cmd.sqlType': 'proc',
				'cmd.QDEPT_NAME': e
			}).then(res => {
				this.uniList = res[0].LIST
			}).catch(err => {
				console.log(err)
			})
		},
		changeUnion(e) {
			this.$api.getData({
				'cmd.sqlKey': 'MSP_WX_LHC.SEL_SELECT',
				'cmd.sqlType': 'proc',
				'cmd.QTYPE': e.value
			}).then(res => {
				this.postList = res[0].LIST2,
				// 清空岗位变量
				this.form.postValue = ''
			}).catch(err => {
				console.log(err)
			})
		},
		changeUnionNew(e) {
			this.uniName = e.DEPT_NAME
			this.form.uniValue = e.ID
			console.log(this.uniName)
			this.$api.getData({
				'cmd.sqlKey': 'MSP_WX_LHC.SEL_SELECT',
				'cmd.sqlType': 'proc',
				'cmd.QTYPE': e.ID
			}).then(res => {
				this.postList = res[0].LIST2,
				// 清空岗位变量
				this.form.postValue = ''
			}).catch(err => {
				console.log(err)
			})
		},
		radioChangeOne(value) {
			this.form.oneCurrent = Number(value.detail.value)
		},
		radioChangeTwo(value) {
			this.form.twoCurrent = Number(value.detail.value)
		},
		radioChangeThree(value) {
			this.form.threeCurrent = Number(value.detail.value)
		},
		radioChangeFour(value) {
			this.form.fourCurrent = Number(value.detail.value)
		},
		submitForm() {
			if(!this.$store.state.hasLogin) {
				uni.showToast({
					icon:"none",
					title:'请先登录！'
				})
				setTimeout(function() {
					uni.reLaunch({
						url: '/pages/user/index'
					})
				}, 1000)
				return
			}
			if(this.form.uniValue === '') {
				uni.showToast({
					icon:"none",
					title:'请选择单位名称！'
				})
				return
			}
			if(this.form.postValue === '') {
				uni.showToast({
					icon:"none",
					title:'请选择岗位名称！'
				})
				return
			}
			if(this.form.postValue == '0' && !this.form.postName) {
				uni.showToast({
					icon:"none",
					title:'请输入科室名称或工作人员名称！'
				})
				return
			}
			let reg = /^((0\d{2,3}\d{7,8})|(1[3456789]\d{9}))$/
			if(this.form.number) {
				if(!reg.test(this.form.number)) {
					uni.showToast({
						icon:"none",
						title:'电话号码格式不正确！'
					})
					return
				}
			}
			if(this.form.oneCurrent === '') {
				uni.showToast({
					icon:"none",
					title:'请对人员服务态度进行评价！'
				})
				return
			}else {
				if(this.form.oneCurrent === 4 && !this.form.twoInput) {
					uni.showToast({
						icon:"none",
						title:'第2项请输入其他原因！'
					})
					return
				}
			}
			if(this.form.twoCurrent === '') {
				uni.showToast({
					icon:"none",
					title:'请对人员办事效率进行评价！'
				})
				return
			}else {
				if(this.form.twoCurrent === 4 && !this.form.fourInput) {
					uni.showToast({
						icon:"none",
						title:'第3项请输入其他原因！'
					})
					return
				}
			}
			if(this.form.threeCurrent === '') {
				uni.showToast({
					icon:"none",
					title:'请选择是否碰到吃拿卡要问题！'
				})
				return
			}else {
				if(this.form.threeCurrent === 1 && !this.form.fiveInput) {
					uni.showToast({
						icon:"none",
						title:'第4项请输入详细情况！'
					})
					return
				}
			}
			if(this.form.fourCurrent === '') {
				uni.showToast({
					icon:"none",
					title:'请对该单位的本次服务整体进行评价！'
				})
				return
			}
			let formData = {
				'cmd.sqlKey': 'MSP_WX_LHC.ADD_HOME',
				'cmd.sqlType': 'proc',
				'cmd.QOPEN_ID': this.$store.state.openid,	//openid
				'cmd.QDEPT_ID': this.form.uniValue,	//单位id
				'cmd.QJOB_ID': this.form.postValue,	//岗位id
				'cmd.QTYPE': this.form.postName,	//岗位名称
				'cmd.QRADIO1': this.form.oneCurrent,	//服务态度
				'cmd.QRADIO2': this.form.twoCurrent,	//办事效率
				'cmd.QRADIO3': this.form.threeCurrent,	//吃拿卡要
				'cmd.QRADIO5': this.form.fourCurrent,	//整体评价
				'cmd.QRADIO1_S': this.form.twoInput,	//服务态度其他填写
				'cmd.QRADIO2_S': this.form.fourInput,	//办事效率其他填写
				'cmd.QRADIO3_S': this.form.fiveInput,	//吃拿卡要碰到填写
				'cmd.QOTHER': this.form.sevenInput,	//其他问题反映填写
				'cmd.QPHONE': this.form.number,	//用户手机号
				'cmd.QWX_NAME': this.$store.state.userInfo.nickName,	//用户微信名
				'cmd.QSEX': this.$store.state.userInfo.gender,	//用户性别
				'cmd.QCITY': this.$store.state.userInfo.city,	//用户城市
				'cmd.QS': this.$store.state.userInfo.province,	//用户省份
				'cmd.QRADIO11': this.form.oneInput,	//服务态度不满意时填写
				'cmd.QRADIO22': this.form.threeInput,	//办事效率不满意时填写
				'cmd.QRADIO33': this.form.sixInput,	//整体评价不满意时填写
			}
			this.$api.getData(formData).then(res => {
				if(res[0].DRCS) {
					uni.showModal({
						title: '提示',
						content: `提交成功，你是今日第${res[0].DRCS}位提交问卷的用户！`,
						showCancel: false,
						cancelText: '取消',
						confirmText: '确定',
						success: res => {
							if (res.confirm) {
								//清空数据
								this.uniName = ''
								this.form = {
									uniValue: '',
									postValue: '',
									oneCurrent: '',
									number: '',
									twoCurrent: '',
									threeCurrent: '',
									fourCurrent: '',
									oneInput: '',
									twoInput: '',
									threeInput: '',
									fourInput: '',
									fiveInput: '',
									sixInput: '',
									sevenInput: ''
								}
							}
						}
					})
				}else {
					uni.showToast({
						icon:"none",
						title:'提交失败！'
					})
				}
			}).catch(err => {
				console.log(err)
			})
		}
	}
}
</script>

<style lang="scss" scoped>
.home-panel {
	.title {
		margin: 38rpx 48rpx;
		font-size: 36rpx;
		color: #0F172A;
		font-family: Poppins;
		font-weight: bold;
		font-size: 32rpx;
		line-height: 48rpx;
		color: #8f8f94;
		.blue {
			color: #506AF0;
		}
	}
	.apply-panel {
		width: 90vw;
		height: 192rpx;
		margin: 32rpx auto;
		display: flex;
		justify-content: center;
		align-items: center;
		border-radius: 20rpx;
		background: #FDF7F2;
		border: 2rpx solid #FFE1CA;
		.icon-panel {
			width: 96rpx;
			height: 96rpx;
		}
		.text-panel {
			margin: 0 32rpx;
			.text-title {
				color: #263343;
				font-family: Source Han Sans CN;
				font-weight: medium;
				font-size: 32rpx;
				line-height: 48rpx;
				text-align: left;
			}
			.text-content {
				color: #64748B;
				font-family: Source Han Sans CN;
				font-weight: regular;
				font-size: 26rpx;
				line-height: 40rpx;
				text-align: left;
			}
		}
		.arrow {
			color: #7D8288;
		}
	}
	.declared-panel {
		padding: 48rpx;
		background: #506AF0;
		color: #FFFFFF;
		font-family: Source Han Sans CN;
		.text-title {
			font-weight: medium;
			font-size: 48rpx;
			line-height: 72rpx;
		}
		.sub-title {
			text-indent: 2em;
			font-weight: medium;
			font-size: 28rpx;
			line-height: 60rpx;
		}
		.right {
			padding-right: 40rpx;
			text-align: right;
		}
		.content-title {
			text-indent: 2em;
			font-weight: regular;
			font-size: 28rpx;
			line-height: 60rpx;
		}
		.text-content {
			font-weight: regular;
			font-size: 28rpx;
			line-height: 50rpx;
		}
	}
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
	.submit-panel-bottom {
		display: flex;
		justify-content: space-around;
		button.one {
			width: 45%;
			color: #ffffff;
			background: #506AF0;
			line-height: 150rpx;
		}
		button.two {
			width: 45%;
			color: #ffffff;
			background: #506AF0;
			line-height: 150rpx;
		}
	}
}
</style>
