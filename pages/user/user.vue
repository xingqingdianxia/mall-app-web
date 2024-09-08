<template>  
    <view class="user-info">
		<view class="user-section">
			<!-- <image class="bg" src="/static/user-bg.jpg"></image> -->
			<view class="user-info-box">
				<view class="portrait-box">
					<image class="portrait" :src="userInfo.icon || '/static/missing-face.png'"></image>
				</view>
				<view class="info-box">
					<text class="username">{{userInfo.nickname || '游客'}}</text>
					<text class="user-account">
						<text class="user-account-id">ID</text>
						<text class="user-account-code">
							<text>2223330</text>
							<text class="iconfont icon-fuzhi user-account-copy"></text>
						</text>
					</text>
				</view>
			</view>
			<view class="user-contant">
				<view class="user-contant-box">
					<view class="flex-item uni-bg-red">
						<view class="user-contant-item">
							<view>
								<text>魔玩币(币)</text>
								<text class="iconfont icon-youjiantou"></text>
							</view>
							<view class="user-contant-item-amount">0.00</view>
						</view>
					</view>
					<view class="flex-item uni-bg-red">
						<view class="user-contant-item">
							<view>
								<text>累计获得(币)</text>
							</view>
							<view class="user-contant-item-amount">0.00</view>
						</view>
					</view>
					<view class="flex-item uni-bg-red">
						<view class="user-contant-item">
							<view>
								<text>累计邀新</text>
							</view>
							<view class="user-contant-item-amount">0</view>
						</view>
					</view>
				</view>
				<view class="user-contant-help">
					<text>如何获得游戏币</text>
					<text class="user-contant-help-icon iconfont icon-bangzhu"></text>
				</view>
			</view>
		</view>
		<!-- 我买到的 -->
		<view class="history-section icon">
			<list-cell icon="iconfont icon-gouwucheman" iconColor="#e07472" title="我买到的" @eventClick="navTo('/pages/address/address')"></list-cell>
			<list-cell icon="iconfont icon-youhuiquan" iconColor="#e07472" title="优惠券" @eventClick="navTo('/pages/user/readHistory')"></list-cell>
		</view>
		<!-- 其他 -->
		<view class="history-section icon">
			<list-cell icon="iconfont icon-kefu" iconColor="#1baaf6" title="联系客服" @eventClick="navTo('/pages/address/address')"></list-cell>
			<list-cell icon="iconfont icon-shenfenrenzheng" iconColor="#1baaf6" title="实名认证" tips="未实名" tipsColor="cell-tip-blue" @eventClick="navTo('/pages/user/readHistory')"></list-cell>
			<list-cell icon="iconfont icon-shezhi" iconColor="#1baaf6" title="设置" @eventClick="navTo('/pages/user/readHistory')"></list-cell>
		</view>
    </view>  
</template>  
<script>  
	import listCell from '@/components/mix-list-cell';
	import {
		fetchMemberCouponList
	} from '@/api/coupon.js';
    import {  
        mapState 
    } from 'vuex';  
	let startY = 0, moveY = 0, pageAtTop = true;
    export default {
		components: {
			listCell
		},
		data(){
			return {
				coverTransform: 'translateY(0px)',
				coverTransition: '0s',
				moving: false,
				couponCount:null
			}
		},
		onLoad(){
		},
		onShow(){
			if(this.hasLogin){
				fetchMemberCouponList(0).then(response=>{
					if(response.data!=null&&response.data.length>0){
						this.couponCount = response.data.length;
					}
				});
			}else{
				this.couponCount=null;
			}
		},
		// #ifndef MP
		onNavigationBarButtonTap(e) {
			const index = e.index;
			if (index === 0) {
				this.navTo('/pages/set/set');
			}else if(index === 1){
				// #ifdef APP-PLUS
				const pages = getCurrentPages();
				const page = pages[pages.length - 1];
				const currentWebview = page.$getAppWebview();
				currentWebview.hideTitleNViewButtonRedDot({
					index
				});
				// #endif
				uni.navigateTo({
					url: '/pages/notice/notice'
				})
			}
		},
		// #endif
        computed: {
			...mapState(['hasLogin','userInfo'])
		},
        methods: {

			/**
			 * 统一跳转接口,拦截未登录路由
			 * navigator标签现在默认没有转场动画，所以用view
			 */
			navTo(url){
				if(!this.hasLogin){
					url = '/pages/public/login';
				}
				uni.navigateTo({  
					url
				})  
			},
        }  
    }  
</script>  
<style lang='scss'>
	/* %flex-center {
	 display:flex;
	 flex-direction: column;
	 justify-content: center;
	 align-items: center;
	}
	%section {
	  display:flex;
	  justify-content: space-around;
	  align-content: center;
	  background: #fff;
	  border-radius: 10upx;
	} */
	
	.user-info {
		background-color: #f3f9fe;
		
		.user-section{
			height: 520upx;
			padding: 100upx 30upx 0;
			position:relative;
			background: linear-gradient(#daf1ff, #f7fcff);
			.bg{
				position:absolute;
				left: 0;
				top: 0;
				width: 100%;
				height: 100%;
				filter: blur(1px);
				opacity: .7;
			}
			.user-info-box{
				height: 180upx;
				display:flex;
				align-items:center;
				position:relative;
				z-index: 1;
				.portrait{
					width: 130upx;
					height: 130upx;
					border:5upx solid #fff;
					border-radius: 50%;
				}
				.info-box {
					display: flex;
					flex-direction: column;
					margin-left: 20upx;
					.username{
						font-size: $font-lg + 6upx;
						color: $font-color-dark;
					}
					.user-account {
						.user-account-id {
							padding: 0 20upx 0 15upx;
							margin-right: 10upx;
							color: #fff;
							background-color: #0498f2;
							border-radius: 100upx;
							font-size: 26upx;
							font-style: italic;
						}
						.user-account-code {
							color: #0498f2;
						}
						.user-account-copy {
							margin-left: 10upx;
							font-size: 38upx;
						}
					}
				}
			}
			.user-contant {
				position: relative;
				.user-contant-box {
					display: flex;
					height: 240upx;
					border-radius: 35upx;
					background-color: #20adf7;
					.flex-item {
						flex: 1;
						color: #fff;
						border-right: 1px solid #35bcfb;
						margin: 40upx 0 80upx 30upx;
						font-size: 28upx;
						&:nth-last-child(1) {
							border-right: none;
						}
						.user-contant-item {
							display: flex;
							flex-direction: column;
							.user-contant-item-amount {
								font-size: 48upx;
							}
						}
					}
				}
				.user-contant-help {
					width: 94%;
					position: absolute;
					bottom: 0upx;
					margin: 20upx;
					padding: 5upx 15upx;
					background-color: #51bdf9;
					border-radius: 40upx;
					font-size: 24upx;
					color: #fff;
					.user-contant-help-icon {
						margin-left: 10upx;
						font-size: 26upx;
					}
				}
			}
		}
		.history-section{
			padding: 30upx 0 0;
			margin-top: 20upx;
			background: #fff;
			border-radius:10upx;
			.sec-header{
				display:flex;
				align-items: center;
				font-size: $font-base;
				color: $font-color-dark;
				line-height: 40upx;
				margin-left: 30upx;
				.yticon{
					font-size: 44upx;
					color: #5eba8f;
					margin-right: 16upx;
					line-height: 40upx;
				}
			}
			.h-list{
				white-space: nowrap;
				padding: 30upx 30upx 0;
				image{
					display:inline-block;
					width: 160upx;
					height: 160upx;
					margin-right: 20upx;
					border-radius: 10upx;
				}
			}
		}
	}
</style>