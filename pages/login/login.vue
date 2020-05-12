<template>
	<view>
		<!-- #ifdef MP-WEIXIN -->
		<button type="primary" open-type="getUserInfo" @getuserinfo="getUserInfo">使用微信登录</button>
		<!-- #endif -->
	</view>
</template>

<script>
var _self, pageOptions, session_key, openid;
export default {
	data() {
		return {};
	},

	methods: {
		// #ifdef MP-WEIXIN
		getUserInfo: info => {
			console.log(info);
			info = info.mp.detail.userInfo;
			// 与服务器交互将数据提交到服务端数据库
			uni.request({
				url: _self.apiServer + 'member&m=login',
				method: 'POST',
				header: { 'content-type': 'application/x-www-form-urlencoded' },
				data: {
					openid: openid,
					name: info.nickName,
					face: info.avatarUrl
				},
				success: res => {
					console.log(res);
					res = res.data;
					// 登录成功 记录会员信息到本地
					if (res.status == 'ok') {
						uni.showToast({ title: '登录成功' });
						uni.setStorageSync('SUID', res.data.u_id + '');
						uni.setStorageSync('SRAND', res.data.u_random + '');
						uni.setStorageSync('SNAME', res.data.u_name + '');
						uni.setStorageSync('SFACE', res.data.u_face + '');
						// 跳转
						if (pageOptions.backtype == 1) {
							uni.redirectTo({ url: pageOptions.backpage });
						} else {
							uni.switchTab({ url: pageOptions.backpage });
						}
					} else {
						uni.showToast({ title: res.data });
					}
				},
				fail: e => {
					console.log(JSON.stringify(e));
				}
			});
		}
		// #endif
	},
	onLoad: function(options) {
		_self = this;
		pageOptions = options;
		// #ifdef MP-WEIXIN
		// 调用 微信 login 获取 code
		uni.login({
			success: res => {
				uni.request({
					url: _self.apiServer + 'member&m=codeToSession&code=' + res.code,
					success: sessions => {
						// sessions.date 数据格式
						// expires_in:7200
						// openid:"oS6of0V0rdp9nY_BuvCnQUasOHYc"
						// session_key:"87sE2oDD8lc+aDJj0tB6+g=="
						// 获取 unionId
						session_key = sessions.data.session_key;
						openid = sessions.data.openid;
					}
				});
			}
		});
		// #endif
	}
};
</script>
<style></style>
