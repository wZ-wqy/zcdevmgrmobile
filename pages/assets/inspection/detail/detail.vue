<template>
	<view class="wrap">
		<view class="all-card">
			<view class="all-card-top">单据号:{{busid}}</view>
			<view class="all-card-ct">
				<u-table>
					<u-tr>
						<u-th>资产总数</u-th>
						<u-th>正常数量</u-th>
						<u-th>故障数量</u-th>
						<u-th>未巡检数</u-th>
					</u-tr>
					<u-tr>
						<u-td>{{data.cnt||0}}</u-td>
						<u-td>{{data.normalcnt||0}}</u-td>
						<u-td>{{data.faultcnt||0}}</u-td>
						<u-td>{{data.actingcnt||0}}</u-td>
					</u-tr>
				</u-table>
			</view>
		</view>
		<view class="all-card">
			<view class="all-card-top">巡检明细</view>
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
								<view class="type">巡检状态:{{ renderInspectStatus(item.inspectstatus) }}</view>
							</view>
						</view>
					</view>
					<u-empty class="epy" text="无资产数据" v-if="!dataList.length" ></u-empty>
				</scroll-view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				busid: "",
				data: {
					cnt: 0,
					normalcnt: 0,
					faultcnt: 0,
					actingcnt: 0
				},
				dataList: []
			}
		},
		methods: {
			renderInspectStatus(status) {
				if (status == "wait") {
					return "未巡检";
				} else if (status == "success") {
					return "正常";
				} else if (status == "failed") {
					return "故障";
				} else {
					return status;
				}
			},
			onLoad(event) {
				this.busid = event.busid;

				if (!this.$u.test.isEmpty(this.busid)) {
					this.$u.post('/api/zc/resInspection/ext/selectById.do', {
						busid: this.busid
					}).then(res => {
						this.data = res.data;
						this.dataList = res.data.items;
					})
				};


			}

		}
	}
</script>

<style lang="scss">

</style>
