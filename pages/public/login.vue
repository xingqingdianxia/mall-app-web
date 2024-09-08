<template>
	<view class="login-page">
		<view class="back-btn iconfont icon-zuo" @click="navBack"></view>
		<!-- 设置白色背景防止软键盘把下部绝对定位元素顶上来盖住输入框等 -->
		<view class="login-portrait">
			<view class="portrait-box">
				<image class="portrait" src="/static/missing-face.png"></image>
				<image class="welcome" src="../../asset/images/welcome.png"></image>
				<text class="login-tips">未注册手机验证后自动登录</text>
			</view>
			<view class="input-content">
				<view class="input-item">
					<input type="text" v-model="username" placeholder="请输入手机号" maxlength="11"/>
				</view>
				<view class="input-item">
					<input type="text" v-model="password" placeholder="请输入验证码" placeholder-class="input-empty" maxlength="20"
					 password @confirm="toLogin" />
				</view>
			</view>
			<button type="primary" class="confirm-btn" @click="toLogin" :disabled="logining">立即登录</button>
		</view>
		<view class="register-section">
			<radio :value="radio" :checked="radio" class="radio" />
			登录即表示同意
			<text @click="toRegist">《儿童个人信息保护指引》</text>
			<text @click="toRegist">《用户协议》</text>
			以及
			<text @click="toRegist">《隐私政策》</text>
		</view>
	</view>
</template>

<script>
	import {
		mapMutations
	} from 'vuex';
	import {
		memberLogin,memberInfo
	} from '@/api/member.js';
	export default {
		data() {
			return {
				username: '',
				password: '',
				logining: true,
				radio: false
			}
		},
		onLoad() {
			this.username = uni.getStorageSync('username') || '';
			this.password = uni.getStorageSync('password') || '';
		},
		methods: {
			...mapMutations(['login']),
			navBack() {
				uni.navigateBack();
			},
			toRegist() {
				uni.navigateTo({url:'/pages/public/register'});
			},
			async toLogin() {
				this.logining = true;
				memberLogin({
					username: this.username,
					password: this.password
				}).then(response => {
					let token = response.data.tokenHead+response.data.token;
					uni.setStorageSync('token',token);
					uni.setStorageSync('username',this.username);
					uni.setStorageSync('password',this.password);
					memberInfo().then(response=>{
						this.login(response.data);
						uni.navigateBack();
					});
				}).catch(() => {
					this.logining = false;
				});
			},
		},

	}
</script>

<style lang='scss'>
	page {
		background: #fff;
	}

	.login-page {
		padding-top: 115px;
		position: relative;
		width: 100vw;
		height: 100vh;
		overflow: hidden;
		background: linear-gradient(#d2edfd, #f7fcff);
		.login-portrait {
			.portrait-box {
				display: flex;
				justify-content: center;
				align-items: center;
				flex-direction: column;
				.portrait {
					height: 308upx;
					width: 308upx;
				}
				.welcome {
					height: 48upx;
					width: 208upx;
					margin: 20upx 0;
				}
				.login-tips {
					margin-bottom: 20upx;
					color: #8c8c8c;
					font-size: $font-base;
				}
			}
		}
	}

	.wrapper {
		position: relative;
		z-index: 90;
		background: #fff;
		padding-bottom: 40upx;
	}

	.back-btn {
		position: absolute;
		left: 40upx;
		z-index: 9999;
		padding-top: var(--status-bar-height);
		top: 40upx;
		font-size: 40upx;
		color: $font-color-dark;
	}

	.input-content {
		padding: 0 60upx;
	}

	.input-item {
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		justify-content: center;
		padding: 0 30upx;
		height: 120upx;
		margin-bottom: 15upx;
		border-bottom: 1px solid #d1dfeb;

		&:last-child {
			margin-bottom: 0;
		}
		input {
			height: 60upx;
			font-size: $font-base + 2upx;
			color: $font-color-dark;
			width: 100%;
		}
	}

	.confirm-btn {
		width: 630upx;
		height: 76upx;
		line-height: 76upx;
		margin-top: 70upx;
		color: #fff;
		font-size: $font-lg;
	}

	.register-section {
		position: absolute;
		left: 0;
		bottom: 50upx;
		width: 100%;
		padding: 0 50upx;
		font-size: $font-sm+2upx;
		color: $font-color-base;
		text-align: center;
		
		.radio {
			transform:scale(0.7);
		}

		text {
			color: $font-color-spec;
			margin-left: 10upx;
		}
	}
</style>
