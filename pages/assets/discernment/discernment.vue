<template>
	<view class="warp">
		<view style="margin-left:20px;margin-right: 20px;padding-top:30px">			
			<view class="uni-btn-v" style="text-align: center;margin-bottom:30px;">
				<u-icon name="scan" :size="180" @click="scan" color="#4790EF"></u-icon>
			</view>
			<view class="uni-title">扫码结果：</view>
				<view class="uni-list" v-if="result">
					<view class="uni-cell">
						<view class="scan-result">
							{{result}}
						</view>
					</view>
					<view class="all-card">
						<view class="all-card-top">基本信息</view>
						<view class="all-card-ct">
							<u-form :model="form"  label-align="left" label-width=150 ref="uForm">
								
								<u-form-item label="资产类别">
									<u-input :disabled="inputdisable" input-align="right" value="form.classfullname" v-model="form.classfullname" placeholder="" />
								</u-form-item>
								<u-form-item label="资产编号">
									<u-input :disabled="inputdisable" input-align="right" value="" v-model="form.uuid" placeholder="" />
								</u-form-item>
								<u-form-item label="规格型号" prop="model">
									<u-input :disabled="inputdisable" input-align="right" value="" v-model="form.model" placeholder="" />
								</u-form-item>
								<u-form-item label="序列" prop="sn">
									<u-input :disabled="inputdisable" input-align="right" value="" v-model="form.sn" placeholder="" />
								</u-form-item>
								<u-form-item label="资产状态" prop="recyclestr">
									<u-input :disabled="inputdisable" input-align="right" value="" v-model="form.recyclestr" placeholder="" />
								</u-form-item>
								<u-form-item label="来源" prop="zcsourcestr">
									<u-input :disabled="inputdisable" input-align="right" value="" v-model="form.zcsourcestr" placeholder="" />
								</u-form-item>
								<u-form-item label="供应商" prop="wbsupplierstr">
									<u-input :disabled="inputdisable" input-align="right" value="" v-model="form.wbsupplierstr" placeholder="" />
								</u-form-item>
								<u-form-item label="品牌" prop="brandstr">
									<u-input :disabled="inputdisable" input-align="right" value="" v-model="form.brandstr" placeholder="" />
								</u-form-item>
								<u-form-item label="使用年限" prop="fs20">
									<u-input :disabled="inputdisable" input-align="right" value="" v-model="form.fs20" placeholder="" />
								</u-form-item>
								<u-form-item label="使用公司" prop="comp_fullname">
									<u-input :disabled="inputdisable" input-align="right" value="" v-model="form.comp_fullname" placeholder="" />
								</u-form-item>
								<u-form-item label="使用部门" prop="part_fullname">
									<u-input :disabled="inputdisable" input-align="right" value="" v-model="form.part_fullname" placeholder="" />
								</u-form-item>
								<u-form-item label="使用人" prop="used_username">
									<u-input :disabled="inputdisable" input-align="right" value="" v-model="form.used_username" placeholder="" />
								</u-form-item>
								<u-form-item label="区域" prop="locstr">
									<u-input :disabled="inputdisable" input-align="right" value="" v-model="form.locstr" placeholder="" />
								</u-form-item>
								<u-form-item label="位置" prop="locdtl">
									<u-input :disabled="inputdisable" input-align="right" value="" v-model="form.locdtl" placeholder="" />
								</u-form-item>
								<u-form-item label="备注" prop="mark">
									<u-input :disabled="inputdisable" input-align="right" value="" v-model="form.mark" placeholder="" />
								</u-form-item>
							</u-form>
						</view>
					</view>
			 
				</view>
        	</view>
			
			
			
		</view>
	</view>
</template>
<script>
	import permision from "@/common/permission.js";
	export default {
		data() {
			return {
				inputdisable:true,
				title: 'scanCode',
				result: '',
				devinfo:'',
				form: {
					classfullname: '',
					recyclestr:"",
					uuid:"",
					model:"",
					supplierstr:"",
					brandstr:"",
					comp_fullname:"",
					part_fullname:"",					
					used_username:"",
					zcsourcestr:"",			
					wbsupplierstr:"",
					wbstr:"",	
					locstr:"",
					locdtl:"",
					buy_timestr:"",
					ip:"",
					envstr:"",
					sn:"",
					mark:"",
					wbout_datestr:"",
					wbstr:"",
					wbsupplierstr:"",
					wbct:"",
					usefullifestr:"",
					fs20:"",
					label1:"",
					label2:""
				},
				disabled:true,
				border: false,
				check: false,
				labelPosition: 'left'
			}
		},
	 
		methods: {
			async scan() {
				// #ifdef APP-PLUS
				let status = await this.checkPermission();
				if (status !== 1) {
				    return;
				}
				// #endif
				uni.scanCode({
					success: (res) => {
						
						
						this.result = res.result;
						this.$u.get('/api/base/res/queryResAll.do', {
							
							uuid:this.result
						}).then(res => {
							console.log(res);
							if(res.data.length==1){
								this.form=res.data[0];
							}
						})
					},
					fail: (err) => {
						// #ifdef MP
						uni.getSetting({
							success: (res) => {
								let authStatus = res.authSetting['scope.camera'];
								if (!authStatus) {
									uni.showModal({
										title: '授权失败',
										content: '需要使用您的相机，请在设置界面打开相关权限',
										success: (res) => {
											if (res.confirm) {
												uni.openSetting()
											}
										}
									})
								}
							}
						})
						// #endif
					}
				});
			},
			// #ifdef APP-PLUS
			async checkPermission(code) {
				let status = permision.isIOS ? await permision.requestIOS('camera') :
					await permision.requestAndroid('android.permission.CAMERA');
				if (status === null || status === 1) {
					status = 1;
				} else {
					uni.showModal({
						content: "需要相机权限",
						confirmText: "设置",
						success: function(res) {
							if (res.confirm) {
								permision.gotoAppSetting();
							}
						}
					})
				}
				return status;
			}
			// #endif
		}
	}
</script>

<style scoped lang="scss">
 
 
.scan-result {
		min-height: 50upx;
		line-height: 50upx;
	}
</style>
 