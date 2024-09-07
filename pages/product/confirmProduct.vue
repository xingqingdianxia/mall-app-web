<template>
	<view class="container">
		<!-- 头 -->
		<view style="height: 12px;"></view>
		<view class="product-title">
			<view class="product-game-info">
				<image class="game-image" :src="options.gamePic" mode="aspectFill"></image>
				<text class="game-name">{{ fullTitleName }}</text>
			</view>
			<view class="product-game-info">
				<image class="game-product-image" :src="options.productPic" mode="aspectFill"></image>
				<view class="game-product-info">
					<view>{{options.productName}}</view>
					<view>¥{{parseFloat(options.promotPrice).toFixed(2)}}</view>
				</view>
			</view>
		</view>
		<!-- 优惠券信息晚点处理 -->
		<view class="attribute-container">
			<view class="game-name" style="font-size: 18px;">填写账户信息</view>
			<view class="attribute-content">
				<view v-for="(item,index) in attribute" class="attribute-content-item">
					<view style="font-size: 18px;">{{item.name}}</view>
					<view>
						<select class="attribute-content-item-unit" v-if="item.inputType === 1" v-model="item.value">
							<option v-for="option in item.inputList.split(',')" :key="option" :value="option">
								{{ option }}</option>
						</select>
						<input class="attribute-content-item-unit" v-else type="text" v-model="item.value" />
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import {
		fetchProductDetail
	} from '@/api/product.js';
	export default {
		data() {
			return {
				product: undefined,
				options: undefined,
				fullTitleName: undefined,
				quantity: 1,
				attribute: []
			}
		},
		async onLoad(options) {
			let id = options.id;
			this.options = options
			this.fullTitleName = this.options.gameName + "/" + JSON.parse(options.spData).map(item => item.value).join(
				'/')
			this.loadData(id);
		},
		methods: {
			loadData(id) {
				fetchProductDetail(id).then(response => {
					this.product = response.data.product;
					this.attribute = response.data.productAttributeList.filter(attr => {
						return attr.type == 1
					})
					console.log(this.attribute)
				});
			}
		}
	}
</script>

<style>
	.container {
		background-color: rgb(243, 249, 254);
		/* 为整个容器添加背景图 */
		/* background-image: url('/static/bg.png'); */
		background-size: cover;
		/* 背景图片覆盖整个容器 */
		background-position: top center;
		background-repeat: no-repeat;
		height: 100vh;
		/* 确保背景图能覆盖整个视口高度 */
	}

	.attribute-container {
		border-radius: 10px;
		padding: 16px;
		background-color: white;
		margin: 34rpx 20upx;
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		justify-content: flex-start;
		border: 1px solid #f5f5f5;
	}

	.product-title {
		background: linear-gradient(180deg, #d3e4ff, #ffffff 40%);
		/* 渐变背景色 */
		border-radius: 10px;
		padding: 16px;

		margin: 0 20upx;
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		justify-content: flex-start;
		border: 1px solid #f5f5f5;
		/* 正确的颜色代码和边框样式 */
	}

	.product-game-info {
		margin: 12px 2px;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		height: 50px;
	}

	.game-image {
		width: 44px;
		height: 44px;
		border-radius: 10px;
		margin-right: 15px;
	}

	.game-name {
		font-size: 16px;
		font-weight: bold;
		color: #333;
		flex-grow: 1;
	}

	.game-product-image {
		width: 54px;
		height: 54px;
		border-radius: 10px;
		margin-right: 15px;
	}

	.game-product-info {
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		justify-content: space-between;
		height: 100%;
	}

	.attribute-content {
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		justify-content: space-between;
		margin-top: 12px;
		width: 100%;
	}

	.attribute-content-item {
		margin: 8px 0;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-between;
		width: 100%;
	}

	.attribute-content-item-unit {
		width: 200px;
		/* 你可以根据需要调整宽度 */
		/* padding: 8px; */
		height: 38px;
		border-radius: 4px;
		background-color: #f4f5f9;
	}
</style>