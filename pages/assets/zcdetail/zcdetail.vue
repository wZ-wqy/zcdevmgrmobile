<template>
	<view class="wrap">
		<view class="all-card">
			<view class="all-card-top">基本信息</view>
			<view class="all-card-ct">
				<u-form :model="form" label-align="left" label-width=150 ref="uForm1">
					<u-form-item label="资产类别">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.classfullname" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="资产编号">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.uuid" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="规格型号">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.model" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="序列">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.sn" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="资产状态">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.recyclestr" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="来源">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.zcsourcestr" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="供应商">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.wbsupplierstr" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="品牌">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.brandstr" placeholder=" " />
					</u-form-item>
					
				 	<u-form-item label="所属公司">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.belongcomp_name" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="使用公司">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.comp_fullname" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="使用部门">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.part_fullname" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="使用人">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.used_username" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="区域">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.locstr" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="位置">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.locdtl" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="备注">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.mark" placeholder=" " />
					</u-form-item>  
				</u-form>
			</view>
		</view>



 
 	<view class="all-card">
			<view class="all-card-top">维保信息</view>
			<view class="all-card-ct">
				<u-form :model="form" label-align="left" label-width="150" ref="uForm3">
					<u-form-item label="维保商">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.mark" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="维保状态">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.wbstr" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="脱保日期">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.wbout_datestr" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="维保说明">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.wbct" placeholder=" " />
					</u-form-item>
				</u-form>
			</view>
		</view>
 
 
		<view class="all-card">
			<view class="all-card-top">其他信息</view>
			<view class="all-card-ct">
				<u-form :model="form" label-align="left" label-width=150 ref="uForm4">
					<u-form-item label="其他编号">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.fs20" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="标签1">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.label1" placeholder=" " />
					</u-form-item>
					
					<u-form-item label="标签2">
						<u-input :disabled="inputdisable" input-align="right" v-model="form.label2" placeholder=" " />
					</u-form-item>
				</u-form>
			</view>
		</view> 
 
	</view>
</template>

<script>
	export default {
		data() {
			return {
				inputdisable: true,
				form: {
					classfullname: "",
					recyclestr: "",
					uuid: "",
					model: "",
					supplierstr: "",
					brandstr: "",
					comp_fullname: "",
					part_fullname: "",
					used_username: "",
					zcsourcestr: "",
					wbsupplierstr: "",
					wbstr: "",
					locstr: "",
					locdtl: "",
					buy_timestr: "",
					ip: "",
					envstr: "",
					sn: "",
					mark: "",
					wbout_datestr: "",
					wbstr: "",
					wbsupplierstr: "",
					wbct: "",
					usefullifestr: "",
					fs20: "",
					label1: "",
					label2: ""
				}
			};
		},
		onLoad(event) {
			var id = event.id;
			if (this.$u.test.empty(id)) {
				id = "";
			} else {
				this.$u.get('/api/base/res/queryResAllById.do', {
					id: id
				}).then(res => {
					if (res.success) {
						this.form = res.data.data;
					} else {}
				})
			}
		},
		computed: {},
		onReady() {},
		methods: {}
	};
</script>

<style>

</style>
