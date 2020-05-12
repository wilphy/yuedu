<template>
	<view>write</view>
</template>

<script>
var loginRes;
export default {
	data() {
		return {};
	},
	onLoad: function() {
		loginRes = this.checkLogin('../write/write', 2);
		if (!loginRes) return;
	}
};
</script>

<style></style>
