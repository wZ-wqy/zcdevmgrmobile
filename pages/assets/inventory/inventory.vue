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
							<u-empty class="epy" text="无数据" v-if="!list.length" mode="data"></u-empty>
						</view>
						<view v-for="(item, index) in list" :key="index">
							<u-card :title="item.remark" :sub-title="item.code" sub-title-color="green" :thumb="item.thumb">
								<view class="" slot="body">
									<u-table>
										<u-tr>
											<u-th>资产总数</u-th>
											<u-th>正常数量</u-th>
											<u-th>故障数量</u-th>
											<u-th>未巡检数</u-th>
										</u-tr>
										<u-tr>
											<u-td>12</u-td>
											<u-td>12</u-td>
											<u-td>22</u-td>
											<u-td>22</u-td>
										</u-tr>
									</u-table>
								</view>
								<view class="" slot="foot">创建时间:{{item.createdOn}}</view>
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
				tabs: [{
					name: '未盘点'
				}, {
					name: '已盘点',
				}],
				swiperCurrent:0,
				current: 0,
				model: {},
				list2: [],
				list: []
			}
		},
		onLoad() {
			this.list = [{
					"createdOn": "2020-02-01",
					"code": "未巡检",
					"remark": "本次长裤数据中心设备巡检",
				},
				{
					"createdOn": "2020-02-01",
					"code": "未巡检",
					"remark": "remark",
				}, {
					"createdOn": "2020-02-01",
					"code": "未巡检",
					"remark": "remark",
				}, {
					"createdOn": "2020-02-01",
					"code": "未巡检",
					"remark": "remark",
				}, {
					"createdOn": "2020-02-01",
					"code": "未巡检",
					"remark": "remark",
				}, {
					"createdOn": "2020-02-01",
					"code": "未巡检",
					"remark": "remark",
				}, {
					"createdOn": "2020-02-01",
					"code": "未巡检",
					"remark": "remark",
				}, {
					"createdOn": "2020-02-01",
					"code": "未巡检",
					"remark": "remark",
				}, {
					"createdOn": "2020-02-01",
					"code": "未巡检",
					"remark": "remark",
				}, {
					"createdOn": "2020-02-01",
					"code": "未巡检",
					"remark": "remark",
				}, {
					"createdOn": "2020-02-01",
					"code": "未巡检",
					"remark": "remark",
				}
			]
		},
		methods: {
			transition(){
			},
			animationfinish(){
			},
			change(index) {
				this.current = index;
			}
		}
	}
</script>

<style>
	 
</style>
