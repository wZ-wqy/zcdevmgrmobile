<template>
	<view class="about">
		<view class="content">
			<view class="qrcode">
	<!-- 			<image src="../../static/freshzhou1983.jpg" @longtap="save"></image> -->
				<text class="tip">QQ群号:904754434</text>
			</view>
			<view class="desc">
				<text class="code">设备资产管理移动端</text>
				是一个使用 <text class="code">uni-app</text> 开发的微信小程序、APP等多端支持的电子设备、固定资产类管理工具。
			</view>
			<view class="source">
				<view class="title">该管理系统后台是：</view>
				<view class="source-list">
					<view class="source-cell">						
						<text>基于Java、Mysql开发</text>
					</view>					
				</view>
			</view>
			
			<view class="source">
				<view class="title">开源地址：</view>
				<view class="source-list">
					<view class="source-cell">						
						<text>https://gitee.com/lank/zcdevmgr</text>
					</view>					
				</view>
			</view>
			 
		</view>
		 
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
	page,
	view {
		display: flex;
	}

	page {
		min-height: 100%;
		background-color: #FFFFFF;
	}

	image {
		width: 360upx;
		height: 360upx;
	}

	.about {
		flex-direction: column;
		flex: 1;
	}

	.content {
		flex: 1;
		padding: 30upx;
		flex-direction: column;
		justify-content: center;
	}

	.qrcode {
		display: flex;
		align-items: center;
		flex-direction: column;
	}

	.qrcode .tip {
		margin-top: 20upx;
	}

	.desc {
		margin-top: 30upx;
		display: block;
	}

	.code {
		color: #e96900;
		background-color: #f8f8f8;
	}

	button {
		width: 100%;
		margin-top: 40upx;
	}

	.version {
		height: 80upx;
		line-height: 80upx;
		justify-content: center;
		color: #ccc;
	}

	.source {
		margin-top: 30upx;
		flex-direction: column;
	}

	.source-list {
		flex-direction: column;
	}

	.link {
		color: #007AFF;
	}
</style>
