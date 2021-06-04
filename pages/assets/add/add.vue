<template>
	<view class="wrap">
		<view>
			<u-toast ref="uToast" />
		</view>
		<view class="all-card">
			<view class="all-card-top">基本信息</view>
			<view class="all-card-ct">

				<u-calendar max-date="2100-01-01" @change="fdchange" :max-year="maxyear" v-model="fdshow" mode="date"></u-calendar>
				<u-calendar max-date="2100-01-01" @change="wbchange" :max-year="maxyear" v-model="wbshow" mode="date"></u-calendar>
				<u-calendar max-date="2100-01-01" @change="bychange" :max-year="maxyear" v-model="byshow" mode="date"></u-calendar>


				<u-form :model="form" label-align="left" label-width=150 ref="uForm">
					<u-form-item :required="required" label="资产名称">
						<u-input input-align="right" v-model="form.name" placeholder=" " />
					</u-form-item>
					<u-form-item :required="required" label="资产分类" prop="tusedcalss">
						<jPicker input-align="right" class="cont" @sure="pickerChange($event,'class_id')" showKey="name" valKey="id"
						 :options="classList" :showSearch="true" />
					</u-form-item>

					<u-form-item :required="required" label="资产状态" prop="tuseduserid">
						<jPicker input-align="right" class="cont" @sure="pickerChange($event,'recycle')" showKey="name" valKey="value"
						 :options="recycleList" />
					</u-form-item>

					<u-form-item label="其他编号">
						<u-input input-align="right" v-model="form.fs20" placeholder=" " />
					</u-form-item>

					<u-form-item :required="required" label="规格型号">
						<u-input input-align="right" v-model="form.model" placeholder=" " />
					</u-form-item>
					<u-form-item label="序列">
						<u-input input-align="right" v-model="form.sn" placeholder=" " />
					</u-form-item>

					<u-form-item label="供应商">
						<jPicker input-align="right" class="cont" @sure="pickerChange($event,'supplier')" showKey="name" valKey="value"
						 :options="zcsList" />
					</u-form-item>


					<u-form-item label="品牌">
						<jPicker input-align="right" class="cont" :showSearch="true" @sure="pickerChange($event,'brand')" showKey="name"
						 valKey="value" :options="brandList" />
					</u-form-item>


					<u-form-item label="来源">
						<jPicker input-align="right" class="cont" @sure="pickerChange($event,'zcsource')" showKey="name" valKey="value"
						 :options="sourceList" />
					</u-form-item>

					<u-form-item label="使用年限">
						<jPicker input-align="right" class="cont" @sure="pickerChange($event,'usefullife')" showKey="name" valKey="value"
						 :options="uselifeList" />
					</u-form-item>

					<u-form-item label="生产日期">
						<u-input @click="fddateSelect()" input-align="right" :disabled="inputdisable" placeholder="生产日期" v-model="form.fd1str" />
					</u-form-item>

					<u-form-item label="采购日期">
						<u-input @click="bydateSelect()" input-align="right" :disabled="inputdisable" placeholder="采购日期" v-model="form.buy_time_f" />
					</u-form-item>


					<u-form-item label="计量单位">
						<u-input input-align="right" v-model="form.unit" placeholder=" " />
					</u-form-item>
					<u-form-item label="配置描述">
						<u-input input-align="right" v-model="form.confdesc" placeholder=" " />
					</u-form-item>

					<u-form-item label="备注">
						<u-input input-align="right" v-model="form.mark" placeholder=" " />
					</u-form-item>
					<u-form-item label="标签1">
						<u-input input-align="right" v-model="form.fs1" placeholder=" " />
					</u-form-item>

					<u-form-item label="标签2">
						<u-input input-align="right" v-model="form.fs2" placeholder=" " />
					</u-form-item>

					<view class="all-card-top">组织信息</view>
					<u-form-item :required="required" label="所属公司">
						<jPicker input-align="right" @sure="pickerChange($event,'belong_company_id')" v-model="form.belong_company_id"
						 class="cont" showKey="name" valKey="value" :options="compList" />

					</u-form-item>
					<!-- 		
					<u-form-item  :required="required"   label="使用公司"  >
						<jPicker input-align="right" v-model="form.used_company_id" class="cont"  @sure="pickerChange($event,'used_company_id')"  showKey="name" valKey="value"
						:options="compList" />			
					</u-form-item> -->

					<u-form-item label="使用部门">
						<jPicker input-align="right" v-model="form.part_id" class="cont" @sure="pickerChange($event,'part_id')" showKey="name"
						 valKey="value" :options="partList" />
					</u-form-item>
					<u-form-item label="使用人">

						<jPicker input-align="right" v-model="form.used_userid" class="cont" @sure="pickerChange($event,'used_userid')"
						 showKey="name" valKey="value" :options="partuserList" />

					</u-form-item>

					<view class="all-card-top">区域位置</view>
					<u-form-item label="区域">

						<jPicker input-align="right" v-model="form.loc" class="cont" @sure="pickerChange($event,'loc')" showKey="name"
						 valKey="value" :options="locList" />


					</u-form-item>

					<u-form-item label="位置">
						<u-input input-align="right" v-model="form.locdtl" placeholder=" " />
					</u-form-item>


					<view class="all-card-top">财务信息</view>
					<u-form-item label="采购单价">
						<u-input input-align="right" v-model="form.buy_price" placeholder=" " />
					</u-form-item>

					<u-form-item label="资产净值">
						<u-input input-align="right" v-model="form.net_worth" placeholder=" " />
					</u-form-item>


					<view class="all-card-top">维保信息</view>
					<u-form-item label="维保商">
						<jPicker input-align="right" v-model="form.zcwbsupper" class="cont" @sure="pickerChange($event,'zcwbsupper')"
						 showKey="name" valKey="value" :options="wbsList" />
					</u-form-item>

					<u-form-item label="脱保计算">
						<jPicker input-align="right" v-model="form.wb_auto" class="cont" @sure="pickerChange($event,'wb_auto')" showKey="name"
						 valKey="value" :options="tbList" />
					</u-form-item>
					<u-form-item label="脱保日期">
						<u-input @click="wbdateSelect()" input-align="right" :disabled="inputdisable" placeholder="脱保日期" v-model="form.wbout_date_f" />
					</u-form-item>

					<u-form-item label="维保状态">
						<u-input input-align="right" v-model="form.wb" placeholder=" " />
						<jPicker input-align="right" v-model="form.wb" class="cont" @sure="pickerChange($event,'wb')" showKey="name"
						 valKey="value" :options="wbList" />
					</u-form-item>
					<u-form-item label="维保说明">
						<u-input input-align="right" v-model="form.wbct" placeholder=" " />
					</u-form-item>

					<u-form-item label="图片">
						<u-upload @on-remove="removepic" max-count=6 ref="uUpload" :action="action" :file-list="fileList" :before-upload="beforeUpload"></u-upload>
					</u-form-item>


					<u-form-item label="">
					</u-form-item>
					<u-form-item label="">
					</u-form-item>


				</u-form>

			</view>
		</view>
		<view class="caozuo">
			<u-button style='width:50%' @click="save()" shape="circle" :ripple="true" type="primary">提交</u-button>
		</view>
	</view>
</template>

<script>
	import jPicker from "@/components/J-Picker/jPicker.vue";

	export default {
		components: {
			jPicker
		},
		data() {
			return {
				wbshow: false,
				fdshow: false,
				byshow: false,
				maxyear: "2100",
				baseURL: this.ghost + '/api/file/fileupload.do',
				action: this.ghost + '/api/file/fileupload.do',
				inputdisable: true,
				recycleList: [],
				fList: [],
				locList: [],
				classList: [],
				fileList: [],
				uselifeList: [],
				sourceList: [],
				brandList: [],
				sourcelist: [],
				tbList: [],
				wbList: [],
				wbsList: [],
				partuserList: [],
				partList: [],
				compList: [],
				zcList: [],
				zcsList: [],
				required: true,
				defaultKeyword: "",
				keyword: "",
				forbid: '',
				isShowKeywordList: false,
				form: {
					name: "",
					zc_cnt: "1",
					img: "",
					attach: "",
					class_id: "",
					buy_price: "",
					net_worth: "",
					wbct: "",
					recycle: "",
					brand: "",
					wb: "valid",
					belong_company_id: "20",
					used_company_id: "20",
					buy_time_f: "",
					wbout_date_f: "",
					fd1str: "",
					attrvals: [],
					brandname: "",
					classfullname: "",
					recycleSel: "",
					recycleSelname: "",
					uuid: "",
					model: "",
					supplierstr: "",
					supplier: "",
					comp_fullname: "",
					part_fullname: "",
					used_username: "",
					zcsource: "",
					wbsupplierstr: "",
					show: true,
					wbstr: "",
					locstr: "",
					locdtl: "",
					buy_timestr: "",
					ip: "",
					envstr: "",
					sn: "",
					mark: "",
					wbct: "",
					usefullife: "",
					fs20: "",
					label1: "",
					label2: ""

				}
			}
		},
		onLoad(event) {
			//this.init();
			var ps = {
				dicts: "zcusefullife,zcwbcomoute,devbrand,devwb,devdc,devrecycle,zcsource,zcwbsupper,zcsupper",
				parts: "Y",
				partusers: "Y",
				comp: "Y",
				belongcomp: "Y",
				uid: "zcdj",
				classroot: 3
			}

			this.$u.post('/api/ctCategroy/queryCategoryTreeList.do', {
				root: 3
			}).then(res => {
				if (res.success) {
					this.classList = res.data;
				}  
			});

			this.$u.post('/api/zc/queryDictFast.do', ps).then(res => {
			 
				if (res.success) {
					this.recycleList = res.data.devrecycle;
					this.brandList = res.data.devbrand;
					this.sourceList = res.data.zcsource;
					this.uselifeList = res.data.zcusefullife;
					this.locList = res.data.devdc;
				//	this.classList = [];
					this.compList = res.data.comp;
					this.partList = res.data.parts;
					this.partuserList = res.data.partusers;
					this.wbList = res.data.devwb;
					this.tbList = res.data.zcwbcomoute;
					this.wbsList = res.data.zcwbsupper;
					this.zcsList = res.data.zcsupper;

				} else {
					this.showMsg(res.message, 0)
				}



			});




		},
		methods: {
			//以上多个change，按我的风格直接合并
			//行内直接调用 @sure="pickerChange($event,'locationVal')" 
			pickerChange(e, key) {
				this.form[key] = e.pickerVal;
			},
			change(val) {
				console.log(val)
			},
			init() {
				this.loadDefaultKeyword();
			},
			fdchange(obj) {
				this.form.fd1str = obj.result;
			},
			fddateSelect() {
				this.fdshow = true;
			},
			bydateSelect() {
				this.byshow = true;
			},
			bychange(obj) {
				this.form.buy_time_f = obj.result;
			},
			wbdateSelect() {
				this.wbshow = true;
			},
			wbchange(obj) {
				this.form.wbout_date_f = obj.result;
			},

			blur() {
				uni.hideKeyboard()
			},
			//加载默认搜索关键字
			loadDefaultKeyword() {
				//定义默认搜索关键字，可以自己实现ajax请求数据再赋值,用户未输入时，以水印方式显示在输入框，直接不输入内容搜索会搜索默认关键字
				this.defaultKeyword = "请选择资产分类";
			},


			//顶置关键字
			setKeyword(index) {
				this.keyword = this.keywordList[index].keyword;
			},
			//清除历史搜索
			oldDelete() {
				uni.showModal({
					content: '确定清除历史搜索记录？',
					success: (res) => {
						if (res.confirm) {
							console.log('用户点击确定');
							this.oldKeywordList = [];
							uni.removeStorage({
								key: 'OldKeys'
							});
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			//热门搜索开关
			hotToggle() {
				this.forbid = this.forbid ? '' : '_forbid';
			},
			doClose() {
				this.isShowKeywordList = false;

			},

			doShow() {
				this.isShowKeywordList = true;

			},
			removepic(index, lists, name) {
				this.fList.splice(index, 1);
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
			//执行搜索
			beforeAvatarUpload(index, file) {
				let param = {};
				let dt = this;
				var iid = this.getUuid();
				if (this.fList.length > 0) {
					this.showMsg("仅支持上传单张照片", 0)
					return;
				}
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
			//保存关键字到历史记录
			saveKeyword(keyword) {
				uni.getStorage({
					key: 'OldKeys',
					success: (res) => {
					 
						var OldKeys = JSON.parse(res.data);
						var findIndex = OldKeys.indexOf(keyword);
						if (findIndex == -1) {
							OldKeys.unshift(keyword);
						} else {
							OldKeys.splice(findIndex, 1);
							OldKeys.unshift(keyword);
						}
						//最多10个纪录
						OldKeys.length > 10 && OldKeys.pop();
						uni.setStorage({
							key: 'OldKeys',
							data: JSON.stringify(OldKeys)
						});
						this.oldKeywordList = OldKeys; //更新历史搜索
					},
					fail: (e) => {
						var OldKeys = [keyword];
						uni.setStorage({
							key: 'OldKeys',
							data: JSON.stringify(OldKeys)
						});
						this.oldKeywordList = OldKeys; //更新历史搜索
					}
				});
			},

			change(val) {
				console.log(val)
			},


			save() {
				var ps = {};
				if (this.$u.test.isEmpty(this.form.name)) {
					this.showMsg("输入资产名称", 0)
					return;
				};
				if (this.$u.test.isEmpty(this.form.class_id)) {
					this.showMsg("请选择资产分类", 0)
					return;
				};
				if (this.$u.test.isEmpty(this.form.recycle)) {
					this.showMsg("请选择资产状态", 0)
					return;
				};
				if (this.$u.test.isEmpty(this.form.model)) {
					this.showMsg("输入规格型号", 0)
					return;
				};
				if (this.$u.test.isEmpty(this.form.belong_company_id)) {
					this.showMsg("请选择所属公司", 0)
					return;
				};

				// if (this.$u.test.isEmpty(this.form.used_company_id)) {
				// 	this.showMsg("请选择使用公司",0)
				// 	return;
				// };
				let data = this.form;
				//console.log(data);
				this.saveloading = true;


				for (var i in data) {
					var value = data[i];
					if (value === '' || value === null || value === undefined) {
						delete data[i];
					}

				}


				data.img = this.fList;

				this.$u.post('/api/base/res/addResCustom.do', data).then(ckrs => {
					this.saveloading = false;
					if (ckrs.success) {

						this.$refs.uToast.show({
							title: "保存成功",
							position: "center",
							type: "success",
							icon: true,
							url: ''
						});

					} else {
						this.showMsg(ckrs.message, 0)
					};
				});




			}
		}
	};
</script>
<style lang='scss'>
	.alertArea .pickerTop {
		border-bottom: 1px solid #dddddd;
	}

	.pageAll {
		width: 100vw;
		background-color: #f3f3f3;
		padding: 10px 0;
		font-size: 14px;
		min-height: 100vh;
	}

	.detailTopInfo {
		border-radius: 8px;
		box-shadow: 0 1px 2px #f3f3f3;
		width: 96vw;
		box-sizing: border-box;
		margin: 0 2vw;
		background-color: #ffffff;
		padding: 8px;
		margin-bottom: 8px;


		.oneData {
			border-bottom: 1px solid #d9d7d7;
			padding: 5px 0;
			line-height: 2rem;

			.title {
				width: 40%;
				float: left;
			}

			.cont {
				width: 60%;
				text-align: right;
				min-height: 2rem;
				display: inline-block;
			}

		}

		.oneData:last-child {
			border-bottom: none;
		}

		.gLableTit {
			border-left: 5px solid #20416c;
			margin: 5px 0 10px;
			padding-left: 5px;
			font-size: 16px;
		}
	}
</style>
