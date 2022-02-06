<template>
	<view>
		<swiper class="swiper"  :indicator-dots="true" :autoplay="true" :interval="3000" 
		:circular="true" :duration="1000">
			<swiper-item v-for="(item,index) in goodsDetails.pics" :key="index">
				<image class="swiper-img" :src="item.pics_big" 
				@click="showSwiperImg(index)"></image>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				goods_id:'',
				goodsDetails:{},
			};
		},
		onLoad(opition) {
			this.goods_id=opition.id
			this.goodsDetailsHttp()
		},
		methods:{
			async goodsDetailsHttp(){
				const {data:res}=await uni.$http.get('/api/public/v1/goods/detail',
				{goods_id:this.goods_id})
				if(res.meta.status!==200){
					return uni.$showmsg()
				}
				this.goodsDetails=res.message
			},
			showSwiperImg(index){
				uni.previewImage({
					current:index,
					urls:this.goodsDetails.pics.map(x=>x.pics_big)
				})
			}
		}
	}
</script>

<style lang="scss">
	.swiper{
		height: 750rpx;
	}
.swiper-img{
	height: 100%;
	width: 100%;
}
</style>
