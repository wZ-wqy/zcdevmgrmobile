<template>
	<view>
		<view class="u-flex user-box u-p-l-30 u-p-r-20 u-p-b-30">
			<view class="u-m-r-10">
				<u-avatar :src="pic" size="140"></u-avatar>
			</view>
			<view class="u-flex-1">
				<view class="u-font-18 u-p-b-20">{{partname}}</view>
				<view class="u-font-14 u-tips-color">{{name}}</view>
			</view>
		<!-- 	<view class="u-m-l-10 u-p-10">
				<u-icon name="scan" color="#969799" size="28"></u-icon>
			</view>
			<view class="u-m-l-10 u-p-10">
				<u-icon name="arrow-right" color="#969799" size="28"></u-icon>
			</view> -->
		</view>

		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item icon="account" @click="openPage('pages/my/myassets/myassets')"  title="我的资产"></u-cell-item>
				<u-cell-item icon="account" @click="openPage('pages/my/repair/repair')"  title="我的报修"></u-cell-item>
				<u-cell-item icon="account" @click="openPage('pages/my/myborrow/myborrow')"  title="我的借用"></u-cell-item>
				<u-cell-item icon="order"    title="打印机连接"></u-cell-item>
				<u-cell-item icon="lock"    @click="openPage('pages/my/changepwd/changepwd')" title="修改密码"></u-cell-item>  
				<u-cell-item icon="man"     @click="logout()" title="用户退出"></u-cell-item>
			</u-cell-group>
		</view>
		<view class="u-m-t-20">
			<u-cell-group>
				<u-cell-item icon="setting"  @click="openPage('pages/my/about/about')" title="关于我们"></u-cell-item>
				<u-cell-item icon="order"    @click="openPage('pages/my/agreement/agreement')" title="用户协议"></u-cell-item>
				<u-cell-item icon="share"    @click="openPage('pages/my/reply/reply')" title="意见反馈"></u-cell-item>
			<!-- 	<u-cell-item icon="star"     @click="openPage('pages/assets/inspection/inspection')" title="test"></u-cell-item>
				 -->
				<u-cell-item icon="level"  :arrow="false" title="版本" value="v1.0.0"></u-cell-item>
			</u-cell-group>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				partname:"",
				name:"",
				pic:'https://uviewui.com/common/logo.png',
				show:true
			}
		},
		onLoad() {
				var user=uni.getStorageSync('_user')
				this.name=user.user_info.name
				console.log(user.user_info);
				this.partname="科技部2";
		 
		},
		methods: {
			logout(){
				var dt=this;
				//强制页面重载，跳转到首页
				this.$u.post('/api/user/logout.do', {
				}).then(res => {
					if(res.success){
						uni.reLaunch({
							url: '../login/index'
						});
					}else{
						console.log(res);
						dt.$refs.uToast.show({
							title: res.message,
							position: "center",
							type: "error",
							icon:true,
							url:''
						 });
					}
				})
			},
			openPage(path) {
				this.$u.route({
					url:path,
				});
			},
		}
	}
</script>

<style lang="scss">
page{
	background-color: #ededed;
}

.camera{
	width: 54px;
	height: 44px;
	
	&:active{
		background-color: #ededed;
	}
}
.user-box{
	background-color: #fff;
}
</style>
