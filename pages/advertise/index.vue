<template>
	<scroll-view scroll-y="true" class="scroll-container">
	    
	    	<image :src="imageUrl" class="full-image" mode="widthFix"></image>
	    
	  </scroll-view>
	
</template>

<script>
	export default {
		data() {
			return {
				imageUrl: '' // 保存传递过来的图片 URL
			}
		},
		onLoad(options) {
			// 获取传递的 URL 参数并解码
			this.imageUrl = decodeURIComponent(options.url);
		}
	}
</script>

<style scoped>
	/* 设置图片全屏展示 */
	.scroll-container {
	  height: 100vh;
	  width: 100vw;
	  overflow-y: scroll;
	}
	
	/* 图片样式，确保图片按比例缩放 */
	.full-image {
	  width: 100%;       /* 图片宽度填满屏幕 */
	  height: auto;      /* 高度自动适应 */
	  display: block;    /* 确保图片占满宽度 */
	}
</style>