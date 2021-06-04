<template>
	<view class="wrap">
		<u-navbar title-color="#fff" back-icon-color="#ffffff" :is-fixed="isFixed" :is-back="isBack" :background="background"
		 :back-text-style="{color: '#fff'}" :title="title" :back-icon-name="backIconName" :back-text="backText">
			<view class="slot-wrap" v-if="useSlot">
				<view class="search-wrap" v-if="search">
					<u-search @custom="searchclick" v-model="keyword" action-text="搜索" :show-action="showAction" height="56"
					 :action-style="{color: '#fff'}"></u-search>
				</view>
				<view class="navbar-right" v-if="rightSlot">
					<view class="message-box right-item">
						<u-icon name="chat" size="38"></u-icon>
						<u-badge count="18" size="mini" :offset="[-15, -15]"></u-badge>
					</view>
					<view class="dot-box right-item">
						<u-icon name="calendar-fill" size="38"></u-icon>
						<u-badge size="mini" :is-dot="true" :offset="[-6, -6]"></u-badge>
					</view>
				</view>
				<view class="map-wrap" v-if="custom">
					<u-icon name="map" color="#ffffff" size="24"></u-icon>
					<text class="map-wrap-text"></text>
					<u-icon name="arrow-down-fill" color="#ffffff" size="22"></u-icon>
				</view>
			</view>
			<view class="navbar-right" slot="right" v-if="slotRight">
				<view class="message-box right-item">
					<u-icon name="chat" size="38"></u-icon>
					<u-badge count="18" size="mini" :offset="[-15, -15]"></u-badge>
				</view>
				<view class="dot-box right-item">
					<u-icon name="calendar-fill" size="38"></u-icon>
					<u-badge size="mini" :is-dot="true" :offset="[-6, -6]"></u-badge>
				</view>
			</view>
		</u-navbar>
		<scroll-view scroll-y style="flex: 1;overflow-y: auto;width: 100%;" @scrolltolower="reachBottom">
		<view class="page-box">
			<view class="order" v-for="(item, index) in dataList" :key="item.id">
				<view class="top">
					<view class="left">
						<view class="store">{{ item.classfullname }}</view>
					</view>
					<view class="right">{{ renderRecycle(item)}}</view>
				</view>
				<view class="item">
					<view class="left">
						<u-checkbox-group @change="checkclick(item)">
							<u-checkbox size=60 icon-size=60 v-model="item.checked" :key="item.id" ></u-checkbox>
						</u-checkbox-group>
					</view>
					<view class="content">
						<view class="title u-line-2">型号:{{ item.model }}</view>
						<view class="type">编号:{{ item.uuid }}</view>
						<view class="type">所属公司:{{ item.belongcomp_name }}</view>
					</view>
				</view>
				<view class="bottom">
					<view class="evaluate btn" @click="detail(item.id)">资产详情</view>
				</view>
			</view>
			<u-empty text="无资产数据" v-if="!dataList.length"></u-empty>
				<view style="padding-top:150rpx">
				</view> 
			</view>
		</scroll-view>
		
		 
	 	<view class="caozuo">
			<u-button @click="save()" style='width:50%' shape="circle" :ripple="true" type="primary">确认</u-button>
		</view>  
	</view>
</template>

<script>
	export default {
		data() {
			return {
				datarange:"",
				category:"3",
				selectList: [],
				dataList: [],
				current: 0,
				swiperCurrent: 0,
				tabsHeight: 0,
				dx: 0,
				loadStatus: ['loadmore', 'loadmore', 'loadmore', 'loadmore'],
				list: [],
				title: '',
				backText: '',
				backIconName: 'nav-back',
				right: false,
				showAction: true,
				rightSlot: false,
				useSlot: true,
				scrollHeight: '400px',
				background: {
					'background-image': 'linear-gradient(45deg, rgb(28, 187, 180), rgb(141, 198, 63))'
				},
				isBack: true,
				search: true,
				custom: false,
				isFixed: true,
				keyword: '',
				// #ifdef MP
				slotRight: false,
				// #endif
				// #ifndef MP
				slotRight: false
				// #endif
			}
		},
		computed: {

		},
		components: {
		},
		methods: {
			reachBottom(){
				
			},
			renderRecycle(item){
				let data=item.recyclestr;
				if (item.inprocess == "1") {
				        if (item.inprocesstype == "LY") {
				            data = data + "(领用流程中)";
				        } else if (item.inprocesstype == "TK") {
				            data = data + "(退库流程中)";
				        } else if (item.inprocesstype == "JY") {
				            data = data + "(借用流程中)";
				        } else if (item.inprocesstype == "BF") {
				            data = data + "(报废流程中)";
				        } else if (item.inprocesstype == "DB") {
				            data = data + "(调拨流程中)";
				        } else {
				            data = data + "(流程中)";
				        }
				    }
					return data;
			},
			checkclick(obj) {
				console.log(obj.checked)
				if(obj.checked){
					this.selectList.push(obj);
				}else{
					let rind=-1;
					for(let i=0;i<this.selectList.length;i++){
						if(this.selectList[i].id==obj.id){
							rind=i;
							break;
						}
					}
					if(rind>0){
						this.selectList.splice(rind, 1);
					} 
				}
			},
			onLoad(event) {			
				console.log(event);
				if (!this.$u.test.empty(event.datarange)) {
					this.datarange=event.datarange;
					
				}
			},
			save() {
				uni.$emit("selectAssetsFun", {
					data: this.selectList
				});
				uni.navigateBack({
					delta: 1,
					success: function() {
						let page = getCurrentPages().pop(); //跳转页面成功之后
						if (!page) return;
						//page.onLoad(); //如果页面存在，则重新刷新页面
					}
				});
			},
			detail(id) {
				this.$u.route({
					url: "/pages/assets/zcdetail/zcdetail",
					params: {
						id: id
					}
				})
			},
			searchclick() {
				var dt = this;
				//强制页面重载，跳转到首页
				if ((!this.$u.test.empty(this.keyword)) && this.keyword.length > 1) {
					this.$u.post('/api/base/res/queryResAll.do', {
						search: this.keyword,
						datarange:this.datarange,
						category: this.category
					}).then(res => {
						if (res.success) {
							for (let i = 0; i < res.data.length; i++) {
								//res.data[i].checked = false;
							}
							this.dataList = res.data;
						}  
					})
				} else {
					uni.showToast({
						title: '请输入关键字',
						icon: "none",
						duration: 1500,
						mask: true
					})
				}
			},
			leftChange(index) {},
			searchChange(index) {}
		}
	}
</script>

<style lang="scss" scoped>
 
	.navbar-right {
		margin-right: 24rpx;
		display: flex;
	}

	.search-wrap {
		margin: 0 20rpx;
		flex: 1;
	}

	.right-item {
		margin: 0 12rpx;
		position: relative;
		color: #ffffff;
		display: flex;
	}

	.message-box {}

	.slot-wrap {
		/* 如果您想让slot内容占满整个导航栏的宽度 */
				/* flex: 1; */
				/* 如果您想让slot内容与导航栏左右有空隙 */
				/* padding: 0 30rpx; */
		display: flex;
		align-items: center;
		flex: 1;
	}
 

	 
</style>
