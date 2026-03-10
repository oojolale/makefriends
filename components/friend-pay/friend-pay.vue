<template>
	<view class="sex-block flex-column">
		<view class="remark-li">
			可选择根据省市区来匹配，不选则全国随机
		</view>
		<view class="li flex-row-left">
			<u-icon name="map-fill" color="#000000"></u-icon>
			<text v-if="addrName" class="text2" @click="addrShow = true">{{ addrName }}</text>
			<text v-else class="text1" @click="addrShow = true">请选择地址</text>
		</view>
		<view class="btn flex-row-center" @click="getRandMember()">
			获取一张纸条 ￥1.00
		</view>
		<view class="remark flex-row-center">
			仅需1元，即可邂逅美好的TA
		</view>
		<!-- 	<view class="remark flex-row-center">
			立即随机获取一位好友纸条信息
		</view> -->
		<view class="back-btn flex-row">
			<view @click="backFn">返回上一步</view>
			<view class="right" @click="putMsg">放入一张纸条</view>
		</view>
	
		<u-picker mode="region" v-model="addrShow" :area-code='initAddr' @confirm="getAddrCode"></u-picker>
	    
	</view>
</template>

<script>
	// ----------------------------------------------------------------------------支付使用的的是插件市场的vk-pay，由于他那个是收费的，我没办法放在代码里  按照他的教程进行相关配置就能跑起来 
	const db = uniCloud.database();
	const uid = db.getCloudEnv('$cloudEnv_uid');
	export default {
		props: {
			sex: {
				type: Number,
				default: 1
			}
		},
		data() {
			return {
				
				form1: {
				  openid: ""
				},
				out_trade_no:'',
				alipayAppPayToH5Pay:false,
				noClick: true,
				sexIndex: 1,
				addrShow: false,
				addrName: '',
				addrCode: '',
				initAddr: [],
			}
		},
		mounted() {
			this.sexIndex = this.sex
		},
		// 监听 - 页面每次【显示时】执行(如：前进和返回) (页面每次出现在屏幕上都触发，包括从下级页面点返回露出当前页面)
		onShow() {
			this.vkPay.pageShow = true;
		},
		// 监听 - 页面每次【隐藏时】执行(如：返回)
		onHide() {
			this.vkPay.pageShow = false;
		},
		methods: {
			getAddrCode(addr) {
				var addrName = addr.province.label + addr.city.label + addr.area.label
				this.addrName = addrName
				this.addrCode = addr.area.value
				this.initAddr = [addr.province.value, addr.city.value, addr.area.value]
			},
			
			getRandMember() {
				this.$emit("paysuccess", {
					navIndex: 2,
					sexIndex: this.sexIndex,
					addrCode: this.addrCode
				})
			},
			putMsg() {
				this.$emit("putmsg", {
					navIndex: 3,
					sexIndex: this.sexIndex
				})
			},
			backFn() {
				this.$emit("changenav", 0)
			},
		}
	}
</script>

<style lang="scss" scoped>
	.sex-block {
		padding: 50rpx 0rpx;
		padding-top: 130rpx;
		.remark-li {
			color: $minor-text-color;
			font-size: 24rpx;
			margin-bottom: 10rpx;
		}

		.li {
			width: 90%;
			background-color: #f2edfc;
			border-radius: 200rpx;
			padding: 30rpx;
			margin-top: 30rpx;

			.text1 {
				width: 90%;
				margin-left: 20rpx;
				color: $minor-text-color;
			}

			.text2 {
				width: 90%;
				margin-left: 20rpx;
			}
		}

		.btn {
			width: 90%;
			margin-top: 50rpx;
			background-color: #FFD800;
			padding: 30rpx;
			border-radius: 200rpx;
			font-weight: bold;
		}

		.remark {
			color: $minor-text-color;
			font-size: 24rpx;
			margin-top: 20rpx;
		}

		.back-btn {
			width: 100%;
			color: $minor-text-color;
			font-size: 24rpx;
			margin-top: 50rpx;
			text-decoration: underline;
			justify-content: space-around;

			.right {
				color: $main-text-color;
			}
		}
	}
</style>
