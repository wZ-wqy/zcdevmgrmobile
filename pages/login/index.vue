<template>
	<view class="wraplogin">
		<view>
			<u-toast ref="uToast" />
		</view>
		<view class="top"></view>
		<view class="content">
			<view class="title">欢迎登录管理系统</view>
			<view class="u-demo-area">
				<u-field v-model="user" label="用户名" placeholder="请填写用户名" :type="type">
				</u-field>
				<u-field v-model="pwd" label="密码" type="password" placeholder="请填写密码">
				</u-field>
			</view>
			<!-- </view> -->
			<view class="u-demo-area">
				<u-button @click="btnClick" data-name="3333" :loading="loading" :plain="plain" :shape="shape" :size="size" :ripple="ripple"
				 :hairLine="hairLine" :type="type">登录</u-button>
			</view>
			<view class="alternative">
				<view class="password">手机号登陆</view>
				<view class="issue">遇到问题</view>
			</view>
		</view>
		<view class="buttom">

			<view class="hint">
				登录代表同意
				<text class="link">设备管理用户协议、隐私政策，</text>
				并授权使用您的设备管理账号信息（如昵称、头像、收获地址）以便您统一管理
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {

				hairLine: true,
				type: 'primary',
				size: 'default',
				shape: 'square',
				plain: false,
				ripple: true,
				loading: false,
				user: 'user1',
				pwd: 'oracle'
			}
		},
		computed: {
			inputStyle() {
				let style = {};
				if (this.tel) {
					style.color = "#fff";
					style.backgroundColor = this.$u.color['warning'];
				}
				return style;
			}
		},
		methods: {
			btnClick() {
				var dt = this;
				if (this.$u.test.isEmpty(this.user)) {
					this.$refs.uToast.show({
						title: "输入正确的用户名",
						position: "center",
						type: "error",
						icon: true,
						url: ''

					});
					return;
				};

				if (this.$u.test.isEmpty(this.pwd)) {
					this.$refs.uToast.show({
						title: "输入验证码",
						position: "center",
						type: "error",
						icon: true,
						url: ''

					});
				};

				//强制页面重载，跳转到首页
				this.$u.get('/api/user/login.do', {
					user: this.user,
					pwd: this.pwd,
					type: "username",
					client: "web",
					basePublic: "yes"
				}).then(res => {
					if (res.success) {
						getApp().globalData._token = res.data.token;

						uni.setStorage({
							key: '_user',
							data: res.data
						})
						console.log("put token in storage:", res.data.token);
						uni.setStorage({
							key: '_token',
							data: res.data.token
						})
						uni.reLaunch({
							url: '../assets/index'
						});
					} else {
						dt.$refs.uToast.show({
							title: res.message,
							position: "center",
							type: "error",
							icon: true,
							url: ''
						});
					}
				})



			}
		}
	};
</script>

<style lang="scss" scoped>
	.wraplogin {
		font-size: 28rpx;

		.content {
			width: 600rpx;
			margin: 80rpx auto 0;

			.title {
				text-align: left;
				font-size: 60rpx;
				font-weight: 500;
				margin-bottom: 100rpx;
			}

			input {
				text-align: left;
				margin-bottom: 10rpx;
				padding-bottom: 6rpx;
			}

			.tips {
				color: $u-type-info;
				margin-bottom: 60rpx;
				margin-top: 8rpx;
			}

			.getCaptcha {
				background-color: rgb(253, 243, 208);
				color: $u-tips-color;
				border: none;
				font-size: 30rpx;
				padding: 12rpx 0;

				&::after {
					border: none;
				}
			}

			.alternative {
				color: $u-tips-color;
				display: flex;
				justify-content: space-between;
				margin-top: 30rpx;
			}
		}

		.buttom {
			.loginType {
				display: flex;
				padding: 350rpx 150rpx 150rpx 150rpx;
				justify-content: space-between;

				.item {
					display: flex;
					flex-direction: column;
					align-items: center;
					color: $u-content-color;
					font-size: 28rpx;
				}
			}

			.hint {
				padding: 20rpx 40rpx;
				font-size: 20rpx;
				color: $u-tips-color;

				.link {
					color: $u-type-warning;
				}
			}
		}
	}
</style>
