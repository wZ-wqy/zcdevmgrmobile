<template>
	<view class="wrap">
		<view class="all-card">
			<view class="all-card-top">基本信息</view>
			<view class="all-card-ct">
			 <u-form :model="form" ref="uForm" label-align="left" label-width=150>
			 	<u-form-item label="原密码"  >
			 		<u-input placeholder="请输入原密码" v-model="form.name" />
			 	</u-form-item>
			 	<u-form-item label="新密码" >
			 		<u-input placeholder="请输入新密码" v-model="form.intro" />
			 	</u-form-item>
			 	<u-form-item label="新密码">
			 		<u-input placeholder="请再次输入新密码" v-model="form.intro" />
			 	</u-form-item>
			 </u-form>
			</view>
		</view>
		<view class="caozuo">
			<u-button style='width:50%' @click="submit()" shape="circle" :ripple="true" type="primary"  >提交</u-button>
		</view>
	</view>
 
</template>

<script>
	export default {
		data() {
			return {
				form:{
					
				}
			}
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
