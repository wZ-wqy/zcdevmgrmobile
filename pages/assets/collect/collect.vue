<template>
	<view class="wrap">
		<view class="u-tabs-box">
			<u-tabs-swiper ref="tabs" :list="tabs" :current="current" @change="change" :is-scroll="false" swiperWidth="750"></u-tabs-swiper>
		</view>
		<swiper class="swiper-box" :current="swiperCurrent" @transition="transition" @animationfinish="animationfinish">
			<swiper-item class="swiper-item">
				<scroll-view scroll-y style="height: 100%;width: 100%;" @scrolltolower="reachBottom">
					<view class="page-box">
						<view>
							<u-empty class="epy" text="无数据" v-if="!dataList.length"></u-empty>
						</view>
						<view class="order" v-for="(item, index) in dataList" :key="item.id">
							<view class="top">
								<view class="left">
									<view class="store">{{ item.busuuid }}</view>
								</view>
								<view class="right">{{ renderStatus(item.status) }}</view>
							</view>
							<view class="item">
								<view class="left">
								</view>
								<view class="content">
									<view class="title u-line-2">单据名称:{{ item.name }}</view>
									<view class="type">使用公司:{{ item.tcompfullname }}</view>
									<view class="type">使用部门:{{ item.tpartfullame }}</view>
									<view class="type">使用人:{{ item.usedusername }}</view>
									<view class="delivery-time">领用时间:{{ item.create_time }}</view>
								</view>
							</view>
							<view class="bottom">
<!-- 								<view class="evaluate btn" @click="detail(item.id)">单据详情</view> -->
							</view>
						</view>
					</view>
				</scroll-view>
			</swiper-item>
		</swiper>
		<view class="caozuo">	
			<u-button @click="openAddPage('/pages/assets/collect/add/add')" style='width:50%' shape="circle" :ripple="true" type="primary">新增领用</u-button>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				swiperCurrent:0,
				tabs: [{
					name: '进行中',
					code:'inprogress'
				},{
					name: '已完成',
					code:'finish'
				}],
				current: 0,
				dataList: []
			}
		},
		onLoad() {
			this.loadData(0);
		},
		methods: {
			reachBottom(){
				
			},
			transition(){
			},
			animationfinish(){
			},
			loadData:function(index){
				this.$u.post('/api/zc/resCollectionreturn/ext/myList.do', {
					bustype:"LY",statustype:this.tabs[index].code
				}).then(res => {
					if(res.success){
						this.dataList=res.data;
					}else{
					}
				})
			},
			renderStatus(status){
				if(status=="apply" ||status=="submitforapproval"){
					return "待送审"
				}else if(status=="finish" ){
					return "流程结束"
				}else if(status=="finish_na" ){
					return "办理完成(未审批)"
				}else if(status=="cancel" ){
					return "取消"
				}else if(status=="failed" ){
					return "审批失败"
				}else if(status=="success" ){
					return "审批成功"
				}else if(status=="running" ||status=="inapproval"){
					return "审批中"
				}else{
					return status;
				}
			},
			openAddPage(path){
				this.$u.route({url:path});
			},
			change(index) {
				this.current = index;
				this.loadData(index);
			}
		}
	}
</script>

<style lang="scss" scoped>
 
</style>
