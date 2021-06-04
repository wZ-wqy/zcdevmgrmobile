<template>
	<view class="wrap">
		<view>
			<u-toast ref="uToast" />
		</view>
		<view class="all-card">
			<view class="all-card-top">报修信息</view>
			<view class="all-card-ct">
				<u-form :model="form" label-align="left" label-width=150 ref="uForm">
					<u-form-item label="报修进度">
						<u-checkbox-group max=2 @change="checkboxGroupChange">
							<u-checkbox @change="normalchange('fixing',form.fixing)" size=60 icon-size=60 v-model="form.fixing">维修中</u-checkbox>
							<u-checkbox @change="normalchange('finish',form.finish)" size=60 icon-size=60 v-model="form.finish">已完成</u-checkbox>
						</u-checkbox-group>
					</u-form-item>
					 <u-form-item label="维护人">
					 	<u-input input-align="right" placeholder="请输入维护人"   v-model="form.fprocessuser" />
					 </u-form-item>
					 <u-form-item  label="维护时间">
					 	<u-input input-align="right" placeholder="请输入维护时间" v-model="form.fprocesstime" />
					 </u-form-item>
					 <u-form-item :required="required" label="报修原因">
					 	<u-input input-align="right" placeholder="请输入报修原因"   v-model="form.freason" />
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
	
		<view class="all-card">
			<view class="all-card-top">资产明细</view>
			<view class="all-card-ct">
				<scroll-view scroll-y style="margin-bottom:120px; height:100%;width: 100%;">
					<view class="order" v-for="(item, index) in dataList" :key="item.id">
						<view class="top">
							<view class="left">
								<view class="store">{{ item.classfullname }}</view>
							</view>
							<view class="right">{{ item.recyclestr }}</view>
						</view>
						<view class="item">
							<view class="left">
								<image src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1599994418340&di=8763d31be74a803917fa134fa743afdb&imgtype=0&src=http%3A%2F%2Fa2.att.hudong.com%2F36%2F48%2F19300001357258133412489354717.jpg"
								 mode="aspectFill"></image>
							</view>
							<view class="content">
								<view class="title u-line-2">型号:{{ item.model }}</view>
								<view class="type">编号:{{ item.uuid }}</view>
								<view class="type">所属公司:{{ item.belongcomp_name }}</view>
							</view>
						</view>
						<view class="bottom">
							<view class="evaluate btn" @click="removeitem(item.id,index)">删除</view>
						</view>
					</view>
					<u-empty class="epy" text="无资产数据" v-if="!dataList.length" ></u-empty>
				</scroll-view>
			</view>
		</view>
		
	<view class="caozuo">
		<view style='width:25%;' @click="scan">
			<u-icon name="scan" size="90" color="#4790EF"></u-icon>
		</view>
		<view style='width:25%;' @click="search">
			<u-icon name="search" size="90" color="#4790EF"></u-icon>
		</view>
		<u-button style='width:50%' @click="save()" shape="circle" :ripple="true" type="primary" :loading="saveloading">提交</u-button>
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
				dataList:[],
				sdataList:[],
				form: {
					busid: "",
					mark: "",
					uuid: "",
					fixing: false ,
					finish: true
				} 
			}
		},
		onLoad(event) {
		 
		},
		onUnload(event){
			uni.$off('selectAssetsFun');
		},
		onShow(event) {
			uni.$on("selectAssetsFun", res => {
	 
				if (res.data.length > 0) {
					this.$refs.uToast.show({
						title: "已选择数量:"+res.data.length,
						position: "center",
						type: "success",
						icon:true,
						url:''
					 });
					this.sdataList = res.data;
					for (let i = 0; i < this.sdataList.length; i++) {
					 
						let newobj = true;
						for (let j = 0; j < this.dataList.length; j++) {
							if (this.dataList[j].id == this.sdataList[i].id) {
								newobj = false;
								break;
							}
						}
						if (newobj) {
						 
							this.dataList.push(this.sdataList[i])
						}
					}
					
				 
				}				
			})
		},
		 
		methods: {
			removeitem(index){
				this.dataList.splice(index, 1);
			},
			search(){
				this.sdataList = [];
				this.$u.route({
					url: "/pages/common/searchassets/searchassets",
					params: {
						datarange: "repair"
					}
				});
			},
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
				if (type == "fixing") {
					if (status) {
						this.form.finish = false;
					} else {
						this.form.finish = true;
					}
				}
				if (type == "finish") {
					if (status) {
						this.form.fixing = false;
					} else {
						this.form.fixing = true;
					}
				}

			},
			clear() {
				 
			},
			save() {
				var ps = this.form;
				 //"2778480A5E140E4A1C227182AF7B8EB8#5C35A56EDE9263B14AD5329DE03121C4#"
				
				if (this.form.finish) {
					ps.fstatus = "finish";
				} else {
					ps.fstatus = "underrepair";
				}
			 
				if (this.$u.test.isEmpty(this.form.freason)) {
					this.$refs.uToast.show({
						title: "请输入报修原因",
						position: "center",
						type: "error",
						icon: true,
						url: ''
					});
					return;
				};
				if(this.dataList.length==0){
					this.$refs.uToast.show({
						title: "请选择报修的资产",
						position: "center",
						type: "error",
						icon: true,
						url: ''
					});
					return;
				}
				
				ps.items=JSON.stringify(this.dataList);
				let fstr="";
				if(this.fList.length>0){
					for(let i=0;i<this.fList.length;i++){
							fstr=fstr+"#"+this.fList[i];
					}
				} 
				ps.files=fstr;
		    	this.saveloading=true;	
				this.$u.post('/api/zc/resRepair/ext/insertOrUpdate.do', ps).then(res => {
					this.saveloading=false;
					if (res.success) { 
						this.$refs.uToast.show({
							title: "报修成功",
							position: "center",
							type: "success",
							icon: true,
							url: ''
						});
						uni.navigateBack({
							delta: 1,
							success: function() {
								let page = getCurrentPages().pop(); //跳转页面成功之后
								if (!page) return;
								//page.onLoad(); //如果页面存在，则重新刷新页面
							}
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
