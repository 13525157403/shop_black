<template>
	<view>
		<view class="search">
			<shopSearch @click="goSearchPage"></shopSearch>
		</view>
		<view>
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" 
			:duration="1000" :circular="true">
				<!-- 循环渲染轮播图的 item 项 -->
				<swiper-item v-for="item in swiperList" :key="item.goods_id">
					<navigator :url="'/subpkg/shop_details/shop_details?id='+item.goods_id">
						<image class="swiper-img" :src="item.image_src" 
						mode="heightFix"></image>
					</navigator>
				</swiper-item>
			</swiper>
		</view>
		<view class="home-nav">
			<navigator v-for="(item,index) in navList" :key="index" @click="navClickTo(item)">
				<image :src="item.image_src"></image>
			</navigator>
		</view>
		<view class="floer-list" v-for="(item,index) in floerList" :key="index">
			<image :src="item.floor_title.image_src" class="title-img"></image>
			<view class="img-show">
				<view class="img-show-left" 
				@click="floerNavictTo(item.product_list[0].navigator_url)">
					<image :style="{width:item.product_list[0].image_width+'rpx' }" 
					:src="item.product_list[0].image_src" mode="widthFix" />
				</view>
				<view class="img-show-right">
					<image :style="{width:i.image_width+'rpx' }" 
					v-show="m!==0"
					@click="floerNavictTo(i.navigator_url)"
					v-for="(i,m) in item.product_list" :src="i.image_src" :key="m"
					 />
				</view>
			</view>
			


		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				swiperList: [],
				navList: [],
				floerList: []
			};
		},
		onLoad() {
			this.swiperHttp()
			this.navHttp()
			this.floerListHttp()
		},
		methods: {
			async swiperHttp() {
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/swiperdata');
				if (res.meta.status !== 200) return uni.$showmsg();
				this.swiperList = res.message
			},
			async navHttp() {
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/catitems')
				if (res.meta.status !== 200) return uni.$showmsg();
				this.navList = res.message
			},
			navClickTo({
				name
			}) {
				if (name == '分类') {
					uni.switchTab({
						url: '/pages/caryt/caryt'
					})
				}
			},
			async floerListHttp() {
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/floordata')
				if (res.meta.status !== 200) return uni.$showmsg();
				this.floerList = res.message
			},
			floerNavictTo(query){
				let path='/subpkg/shop_list/shop_list?query='+query.split('=')[1]
				uni.navigateTo({
					url:path
				})
			},
			goSearchPage(){
				uni.navigateTo({
					url:'../../subpkg/shop_search/shop_search'
				})
			}
		},
	}
</script>

<style lang="scss" scoped>
	.swiper-img{
		width: 100%;
	}
	.search{
		position: sticky;
		z-index: 1000;
		top: 0;
	}
	.home-nav {
		width: 100%;
		display: flex;
		justify-content: space-around;
		padding: 15px 0;

		image {
			width: 128rpx;
			height: 128rpx;
			
		}
	}

	.floer-list {}

	.title-img {
		width: 100%;
		height: 60rpx;
	}
	.img-show{
		display: flex;
		.img-show-left{
			margin:0 10rpx;
		}
		.img-show-right{
			display: flex;
			flex-wrap: wrap;
			justify-content: space-around;
			align-content: space-between;
				
			image{
				height: 190rpx;
			}
		}
	}
</style>
