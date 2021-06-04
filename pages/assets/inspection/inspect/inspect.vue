<template>
	<view class="wrap">
		<view>
			<u-toast ref="uToast" />
		</view>
		<view class="all-card">
			<view class="all-card-top">巡检信息</view>
			<view class="all-card-ct">
				<u-form :model="form" label-align="left" label-width=150 ref="uForm">
					<u-form-item :required="required" label="单据号">
						<u-input :disabled="disabled" input-align="right" placeholder="" :value="form.busid" v-model="form.busid" />
					</u-form-item>
					<u-form-item :required="required" label="资产编号">
						<u-input input-align="right" placeholder="请输入资产编号" :value="form.uuid" v-model="form.uuid" />
					</u-form-item>
					<u-form-item label="巡检结果">
						<u-checkbox-group max=2 @change="checkboxGroupChange">
							<u-checkbox @change="normalchange('success',form.status_success)" size=60 icon-size=60 v-model="form.status_success">正常</u-checkbox>
							<u-checkbox @change="normalchange('failed',form.status_failed)" size=60 icon-size=60 v-model="form.status_failed">故障</u-checkbox>
						</u-checkbox-group>
					</u-form-item>
					<u-form-item label="备注">
						<u-input input-align="right" placeholder="请输入备注" :value="form.mark" v-model="form.mark" />
					</u-form-item>
					<u-form-item label="图片">
						<u-upload @on-remove="removepic" max-count=6 ref="uUpload" :action="action" :file-list="fileList" :before-upload="beforeUpload"></u-upload>
					</u-form-item>
				</u-form>
			</view>
		</view>
		
		<view class="caozuo">
			<view style='width:45%;' @click="scan">
				<u-icon name="scan" size="90" color="#4790EF"></u-icon>
			</view>
			<u-button style='width:45%' @click="save()" shape="circle" :ripple="true" type="primary" :loading="saveloading">提交</u-button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				saveloading: false,
				required: true,
				disabled: true,
				baseURL: this.ghost + '/api/file/fileupload.do',
				action: this.ghost + '/api/file/fileupload.do',
				fileList: [],
				fList: [],
				form: {
					busid: "",
					mark: "",
					uuid: "",
					status_success: true,
					status_failed: false
				},
				busid: ""
			}
		},
		onLoad(event) {
			this.form.busid = event.busid;
		},
		methods: {
			checkboxGroupChange(){

			},
			removepic(index, lists, name){
				this.fList.splice(index,1);
			},
			beforeUpload(index, list) {
				this.beforeAvatarUpload(index, list)
			},
			getUuid() {
				var len = 32; // 32长度
				var radix = 16; // 16进制
				var chars = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz'
					.split('');
				var uuid = [],
					i;
				radix = radix || chars.length;
				if (len) {
					for (i = 0; i < len; i++)
						uuid[i] = chars[0 | Math.random() * radix];
				} else {
					var r;
					uuid[8] = uuid[13] = uuid[18] = uuid[23] = '-';
					uuid[14] = '4';
					for (i = 0; i < 36; i++) {
						if (!uuid[i]) {
							r = 0 | Math.random() * 16;
							uuid[i] = chars[(i == 19) ? (r & 0x3) | 0x8 : r];
						}
					}
				}
				return uuid.join('');
			},
			beforeAvatarUpload(index, file) {
				let param = {};
				let dt = this;
				var iid = this.getUuid();
				uni.uploadFile({
					url: this.baseURL + "?uuid=" + iid + "&bus=file&interval=10000", //你上传接口
					filePath: file[index].url, //上传的文件
					name: "file[0]", //后台接收文件的标识
					formData: { //传递的额外参数，只能是objectle类型
						"param": JSON.stringify(param) //自己项目需要
					},
					success: (res) => {
						dt.fList.push(iid);
					},
					
					fail: (err) => {

						this.$refs.uToast.show({
							title: "上传图片失败",
							position: "center",
							type: "error",
							icon: true,
							url: ''
						});
					}
				});
			},


			normalchange(type, status) {
				if (type == "success") {
					if (status) {
						this.form.status_failed = false;
					} else {
						this.form.status_failed = true;
					}
				}
				if (type == "failed") {
					if (status) {
						this.form.status_success = false;
					} else {
						this.form.status_success = true;
					}
				}

			},
			clear() {
				this.fList = [];
				this.fileList = [];
				this.form.mark = "";
				this.form.uuid = "";
				this.form.status_success = true;
				this.form.status_failed = false;
				this.$refs.uUpload.clear();
			},
			save() {
				
				var ps = this.form;
				ps.pics = JSON.stringify(this.fList)
				if (this.form.status_success) {
					ps.status = "success";
				} else {
					ps.status = "failed";
				}
				if (this.$u.test.isEmpty(this.form.uuid)) {
					this.$refs.uToast.show({
						title: "请选择资产",
						position: "center",
						type: "error",
						icon: true,
						url: ''
					});
					return;
				};

				if (this.$u.test.isEmpty(this.form.busid)) {
					this.$refs.uToast.show({
						title: "请输入巡检单据",
						position: "center",
						type: "error",
						icon: true,
						url: ''
					});
					return;
				};

		    	this.saveloading=true;	
				this.$u.post('/api/zc/resInspection/ext/actionInspect.do', ps).then(res => {
					this.saveloading=false;
					if (res.success) {
						this.clear();
						this.$refs.uToast.show({
							title: "巡检成功",
							position: "center",
							type: "success",
							icon: true,
							url: ''
						});
					} else {
						this.$refs.uToast.show({
							title: res.message,
							position: "center",
							type: "error",
							icon: true,
							url: ''
						});
					}
				})
			},
			async scan() {
					// #ifdef APP-PLUS
					let status = await this.checkPermission();
					if (status !== 1) {
						return;
					}
					// #endif
					uni.scanCode({
						success: (res) => {
							this.form.uuid = res.result;
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
				}
				// #ifdef APP-PLUS
				,
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

<style lang="scss" scoped>
 
</style>
