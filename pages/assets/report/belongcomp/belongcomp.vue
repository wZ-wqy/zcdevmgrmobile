<template>
	<view class="wrap">
		<swiper class="swiper-box">
			<swiper-item class="swiper-item">
				<scroll-view scroll-y style="height: 100%;width: 100%;">
					<view class="page-box">
						 <u-card :border="false" style="margin:0px" v-for="(item, index) in dataList" :key="index" :title="item.belongcomp_fullname"   >
						 	<view class="" slot="body"> 
								<u-table style="padding:0px">
									<u-tr>
										<u-th>资产总数</u-th>
									</u-tr>
									<u-tr>
										<u-td>{{item.zc_cnt}}</u-td>
									</u-tr>
								</u-table> 
							</view>
						 </u-card>
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
				dataList:[],
			}
		},
		onLoad(event) {
			this.$u.post('/api/zc/report/queryBelongCompReport.do', {
			}).then(res => {
				if(res.success){
					this.dataList=res.data;
				}else{
				}
			})
		},
		methods: {
			
		}
	}
</script>

<style>
 
	 
</style>
