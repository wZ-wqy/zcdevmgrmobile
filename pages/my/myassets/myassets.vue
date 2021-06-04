<template>
	<view class="wrap">
		<u-empty class="epy" text="无个人资产" v-if="!dataList.length" mode="data"></u-empty>
		<view class="order" v-for="(res, index) in dataList" :key="res.id">
			<view class="top">
				<view class="left">
					<u-icon name="home" :size="30" color="rgb(94,94,94)"></u-icon>
					<view class="store">{{ res.classfullname }}</view>
					<u-icon name="arrow-right" color="rgb(203,203,203)" :size="26"></u-icon>
				</view>
				<view class="right">{{ res.recyclestr }}</view>
			</view>	
			<view class="item" >
				<view class="left"><image src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1599994418340&di=8763d31be74a803917fa134fa743afdb&imgtype=0&src=http%3A%2F%2Fa2.att.hudong.com%2F36%2F48%2F19300001357258133412489354717.jpg" mode="aspectFill"></image></view>
				<view class="content">
					<view class="title u-line-2">型号:{{ res.model }}</view>
					<view class="type">编号:{{ res.uuid }}</view>
					<view class="type">供应商:{{ res.supplierstr }}</view>
					<view class="type">序列:{{ res.sn }}</view>
					<view class="type">使用部门:{{ res.part_fullname }}</view>											
					<view class="type">位置:{{ res.locstr }}</view>
					<view class="delivery-time">购买时间:{{ res.buy_timestr }}</view>
				</view> 
			</view>
			<view class="bottom">
				<view class="evaluate btn" @click="detail(res.id)">资产详情</view>
			</view>
		</view> 
	</view> 
</template>
<script>
	export default {
		data() {
			return {
				dataList: []
			}
		},
		computed: {
			slotRightCurrent() {
				return this.slotRight ? 0 : 1;
			}
		},
		onLoad(event) {
			var dt=this;
			//强制页面重载，跳转到首页
			this.$u.post('/api/zc/my/queryMyResAll.do', {
				start:0,length:200
			}).then(res => {
				if(res.success){
					console.log(res);
					this.dataList=res.data;
				}else{
				}
			})
		},
		methods: {
			detail(id){
				this.$u.route({url:"/pages/assets/zcdetail/zcdetail",params: {
					id: id
				}})
			},
			customclick(){
			
			},
			leftChange(index) {
			},
			searchChange(index) {
			}	 
		}
	}
</script>
 
<style>
	
/* #ifndef H5 */
page {
	height: 100%;
	background-color: #f2f2f2;
}
/* #endif */
.epy{
	padding-top:30px;
}
</style>

 