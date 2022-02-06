<template>
	<view class="search">
		<view class="search-input" @click="eventClick">
			<uni-icons type="search" size="17"></uni-icons>
			<text class="plaer">搜索</text>
		</view>
	</view>
</template>

<script>
	export default {
		name: "shopSearch",
		data() {
			return {

			};
		},
		methods:{
			eventClick(){
				this.$emit('click')
			}
		}
	}
</script>

<style lang="scss" scoped>
	.search {
		height: 50px;
		padding: 0 10px;
		background-color: rgb(216,30,6);
		display: flex;
		justify-content: center;
		align-items: center;
		.search-input{
			width: 100%;
			height: 36px;
			border-radius: 18px;
			background-color: #FFFFFF;
			display: flex;
			font-size: 13px;
			justify-content: center;
			align-items: center;
			.plaer{
				font-size: 15px;
				margin-left: 5px;
			}
			
		}
	}
</style>
