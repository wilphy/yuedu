<template>
	<view>index</view>
</template>

<script>
export default {
	data() {
		return {};
	},
	onLoad() {
		uni.request({
			url: this.apiServer + 'index&m=index',
			method: 'GET',
			data: {},
			success: res => {
				console.log(JSON.stringify(res));
			},
			fail: e => {
				console.log(JSON.stringify(e));
			},
			complete: () => {}
		});
	},
	methods: {}
};
</script>

<style></style>
