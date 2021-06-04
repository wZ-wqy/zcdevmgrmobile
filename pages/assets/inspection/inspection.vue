<template>
	<view class="wrap">
		<view>
			<u-toast ref="uToast" />
		</view>
		<view class="u-tabs-box">
			<u-tabs-swiper ref="tabs" :list="tabs" :current="current" @change="change" :is-scroll="false" swiperWidth="750"></u-tabs-swiper>
		</view>
		<swiper class="swiper-box" :current="swiperCurrent" @transition="transition" @animationfinish="animationfinish">
			<swiper-item class="swiper-item">
				<scroll-view scroll-y style="height: 100%;width: 100%;" @scrolltolower="reachBottom">
					<view class="page-box">
						<view>
							<u-empty class="epy" text="无数据" v-if="!list.length"></u-empty>
						</view>
						<view v-for="(item, index) in list" :key="index">
							<u-card :title="renderName(item)" :sub-title="item.busid" sub-title-color="green" :thumb="item.thumb">
								<view class="" slot="body">
									<view class="renderMethod">
										 {{renderMethod(item.method)}}
									</view>
									<u-table>
										<u-tr>
											<u-th>资产总数</u-th>
											<u-th>正常数量</u-th>
											<u-th>故障数量</u-th>
											<u-th>未巡检数</u-th>
										</u-tr>
										<u-tr>
											<u-td>{{item.cnt||0}}</u-td>
											<u-td>{{item.normalcnt||0}}</u-td>
											<u-td>{{item.faultcnt||0}}</u-td>
											<u-td>{{item.actingcnt||0}}</u-td>
										</u-tr>
									</u-table>
									<view class="btn">
										<u-row gutter="16">
											<u-col span="4">
												<u-button size="mini" :plain="true" @click="detail(item.busid)" type="success">巡检详情</u-button>
											</u-col>
											<u-col span="4" v-show="item.status!='finish' ">
													<u-button  size="mini" :plain="true" @click="start(item.busid)" type="success">开始巡检</u-button>
											</u-col>
											<u-col span="4" v-show="item.status!='finish' ">
													<u-button  size="mini" :plain="true" @click="finish(item.busid)" type="success">结束巡检</u-button>
											</u-col>
										</u-row>
									</view>
								</view>
								<view class="" slot="foot">创建时间:{{item.createTime}}</view>
							</u-card>
						</view>
					</view>
				</scroll-view>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	export default {

		data() {
			return {
				finishloading:false,
				swiperCurrent:0,
				tabs: [
					{
						name: '未巡检',
						code:"inprogress"
					},{
						name: '已巡检',
						code:"finish"
					}
					],
				current: 0,
				list: []
			}
		},
		onLoad() {
			this.loadData(0);
		},
		methods: {
			detail(busid){
				this.$u.route({url:"pages/assets/inspection/detail/detail",params: {
					busid: busid
				}});
			},
			transition(){
			},
			animationfinish(){
			},
			reachBottom(){
				
			},
			start(busid){	
				this.$u.post('/api/zc/resInspection/ext/startInspect.do', {
					 busid:busid
				}).then(res => {
					this.$u.route({url:"pages/assets/inspection/inspect/inspect",params: {
					busid: busid
				}});
				})
			},
			finish(busid){
				this.finishloading=true;
				this.$u.post('/api/zc/resInspection/ext/finishInspect.do', {
					 busid:busid
				}).then(res => {
					this.finishloading=false;
					 let type="error"
					 if (res.success) {
						type="success";
						this.loadData(this.current)
					 } 
						this.$refs.uToast.show({
							title: res.message,
							position: "center",
							type: type,
							icon: true,
							url: ''
						});
				})
			},
			renderMethod(value){
				if(value=="free"){
					return "自由模式"
				}else if(value=="fix"){
					return "固定模式"
				}else{
					return value;
				}
			},
			renderName(item){
				let v=item.name;
				if(item.status=='finish'){
					v=v+"--已完成";
				}else if(item.status=='wait'){
					v=v+"--待巡检";
				}else if(item.status=='acting'){
					v=v+"--巡检中";
				}else{
					v=v+"--"+istem.status;
				}
				return v;
			},
			renderStatus(status){
				if(status=="wait"){
					return "未开始";
				}else if(status=="acting"){
					return "进行中";
				}else if(status=="finish"){
					return "完成";
				}else{
					return status;
				}
			},
			loadData:function(index){
				this.$u.post('/api/zc/resInspection/ext/myList.do', {
					statustype:this.tabs[index].code
				}).then(res => {
					if(res.success){
						this.list=res.data;
					}else{
					}
				})
			},
			change(index) {
				this.current = index;
				this.loadData(index);
			}
		}
	}
</script>

<style lang="scss" scoped>
	.renderMethod{
		margin-left:3rpx;
		margin-top:8rpx;
		color: $u-type-primary;
		margin-bottom:30rpx;
	}
	.btn{
		margin-bottom: 25rpx;
		margin-top:45rpx;
	}
 
	
</style>
