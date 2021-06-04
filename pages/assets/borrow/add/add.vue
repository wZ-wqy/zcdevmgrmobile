<template>
	<view class="wrap">
		<view>
			<u-toast ref="uToast" />
		</view>
		<view class="all-card">
			<view class="all-card-top">填写借用信息</view>
			<view class="all-card-ct">
				<u-select @confirm="userConfirm" mode="single-column" v-model="usershow" :list="userlist"></u-select>
				<u-calendar max-date="2100-01-01" @change="busdatechange" :max-year="maxyear" v-model="jyshow" mode="date"></u-calendar>
				<u-calendar max-date="2100-01-01" @change="returndatechange" :max-year="maxyear" v-model="ghshow" mode="date"></u-calendar>
				<u-form :model="form" label-align="left" label-width=150 ref="uForm">
					<u-form-item :required="required" label="单据名称" prop="name">
						<u-input input-align="right" placeholder="请输入名称" v-model="form.name" />
					</u-form-item>
					<u-form-item :required="required" label="借用时间" prop="busdate">
						<u-input @click="busdateSelect()" input-align="right" :disabled="inputdisable" placeholder="借用时间" v-model="form.busdate" />
					</u-form-item>
					<u-form-item :required="required" label="归还时间" prop="returndate">
						<u-input @click="returndateSelect()" input-align="right" :disabled="inputdisable" placeholder="归还时间" v-model="form.returndate" />
					</u-form-item>
					<u-form-item :required="required" label="借用人" prop="lrusername">
						<u-input @click="userSelect()" :disabled="inputdisable" input-align="right" placeholder="请选择" v-model="form.lrusername" />
					</u-form-item>
					<u-form-item label="备注" prop="mark" v-model="form.mark">
						<u-input input-align="right" placeholder="请输入备注" />
					</u-form-item>
				</u-form>

			</view>
		</view>

		<view class="all-card">
			<view class="all-card-top">借用资产明细</view>
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
	import permision from "@/common/permission.js";
	export default {
		data() {
			return {
				required:true,
				show: true,
				saveloading: false,
				inputdisable: true,
				maxyear: "2100",
				jyshow: false,
				ghshow:false,
				usershow: false,
				sdataList: [],
				dataList: [],
				userlist: [],
				form: {
					bustype: "JY",
					name: "",
					busdate: "",
					lprocessuserid: "",
					lprocessusername: "",
					lruserid: "",
					lrusername: "",
					mark: "",
					returndate:"",
					items: ""
				}
			}
		},
		onLoad(event) {
			var dt = this;
			var ps = {};
			ps.parts = "N";
			ps.partusers = "Y";
			ps.comp = "N";
			ps.uid = "mobile_data2";
			ps.dicts = "";
			dt.$u.post('/api/zc/queryDictFast.do', ps).then(res => {
				if (res.success) {
					dt.userlist = res.data.partusers;
				} else {}
			})
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
			search() {
				this.sdataList = [];
				this.$u.route({
					url: "/pages/common/searchassets/searchassets",
					params: {
						datarange: "JY"
					}
				});
			},
			removeitem(id, index) {
				this.dataList.splice(index, 1);
			},
			save() {
				var ps = {};
				if(this.$u.test.isEmpty(this.form.name)){
					this.$refs.uToast.show({
						title: "输入单据名称",
						position: "center",
						type: "error",
						icon:true,
						url:''
					 });
					return;
				};
				if(this.$u.test.isEmpty(this.form.lruserid)){
					this.$refs.uToast.show({
						title: "选择借用人",
						position: "center",
						type: "error",
						icon:true,
						url:''
					 });
					return;
				};
				if(this.$u.test.isEmpty(this.form.busdate)){
					this.$refs.uToast.show({
						title: "选择借用时间",
						position: "center",
						type: "error",
						icon:true,
						url:''
					 });
					return;
				};
				if(this.$u.test.isEmpty(this.form.returndate)){
					this.$refs.uToast.show({
						title: "选择归还时间",
						position: "center",
						type: "error",
						icon:true,
						url:''
					 });
					return;
				};
				if (this.dataList.length == 0) {
					this.$refs.uToast.show({
						title: "请选择资产",
						position: "center",
						type: "error",
						icon: true,
						url: ''
					});
					return;
				};

				if (this.dataList.length > 10) {
					this.$refs.uToast.show({
						title: "借用资产数量不可超过10",
						position: "center",
						type: "error",
						icon: true,
						url: ''
					});
					return;
				};
				ps = this.form;
				ps.items = JSON.stringify(this.dataList)
				this.saveloading = true;				
				var checkitems=[];
				for(let i=0;i<this.dataList.length;i++){
					checkitems.push(this.dataList[i].id);
				}
				let checkps={type:"JY",items:JSON.stringify(checkitems)};
				this.$u.post('/api/zc/fastProcessItemCheck.do', checkps).then(ckrs => {
					this.saveloading = false;
					if (ckrs.success) {
						 this.$u.post('/api/zc/resLoanreturn/ext/zcjy.do', ps).then(res => {
						 	if (res.success) {
						 		var pps = {
						 			ifsp: "0",
						 			ptype: "JY",
						 			title: "",
						 			busid: res.data.busid,
						 			formtype: "none"
						 		}
						 		this.$u.post('/api/zc/flow/startAssetFlow.do', pps).then(rs => {
						 			if (rs.success) {
						 				this.$refs.uToast.show({
						 					title: '新增成功',
						 					type: 'success',
						 					url: '/pages/user/index'
						 				})
						 				uni.navigateBack({
						 					delta: 1,
						 					success: function() {
						 						let page = getCurrentPages().pop(); //跳转页面成功之后
						 						if (!page) return;
						 						//page.onLoad(); //如果页面存在，则重新刷新页面
						 					}
						 				});
						 			} else {}
						 		})

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
						 			
					} else {
						this.$refs.uToast.show({
							title: ckrs.message,
							position: "center",
							type: "error",
							icon: true,
							url: ''
						});
					}
				})
			},
			userConfirm(obj) {
				this.form.lprocessuserid = obj[0].value;
				this.form.lprocessusername = obj[0].label;
				this.form.lruserid = obj[0].value;
				this.form.lrusername = obj[0].label;
			}, 
			busdatechange(obj) {
				this.form.busdate = obj.result;
			},
			returndatechange(obj) {
				this.form.returndate = obj.result;
			},
			userSelect() {
				this.usershow = true;
			},
			busdateSelect() {
				this.jyshow = true;
			},
			returndateSelect() {
				this.ghshow = true;
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
							this.result = res.result;
							this.$u.get('/api/base/res/queryResAll.do', {
								uuid:this.result
							}).then(res => {
								if(res.data.length==1){
									 let newobj = true;
									 for (let j = 0; j < this.dataList.length; j++) {
										if (this.dataList[j].id == res.data[0].id) {
											newobj = false;
											break;
										}
									 }
									 if (newobj) {
										this.dataList.push(res.data[0])
									 }						 
								}else{
									this.$refs.uToast.show({
										title: "无资产数据",
										position: "center",
										type: "error",
										icon: true,
										url: ''
									});
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
