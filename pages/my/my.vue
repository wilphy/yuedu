<template>
	<view>my</view>
</template>

<script>
var loginRes;
export default {
	data() {
		return {};
	},
	onLoad: function() {
		loginRes = this.checkLogin('../my/my', 2);
		if (!loginRes) return;
	}
};
</script>

<style></style>
