<template>
	<view>
			
		<block v-for="(item,index) in goodShopList" :key="index">
			<view class="shop-list-item" @click="goShopDetails(item)">
				<view class="left-img">
					<image :src="item.goods_big_logo" mode=""></image>
				</view>
				<view class="right-text">
					<view class="right-text-top">
						{{item.goods_name}}
					</view>
					<view class="right-text-bottom">
						￥{{item.goods_price}}
					</view>
				</view>
			</view>
			
		</block>
		<view class="more-no" v-if="ismore">
			暂无更多数据
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				shopListquery:{
					query:'',
					cid:'',
					pagenum:1,
					pagesize:15
				},
				goodShopList:[],
				isloding:false,
				ismore:false
			};
		},
		onLoad(opition) {
			this.shopListquery.query=opition.query||''
			this.shopListquery.cid=opition.id||''
			this.getShopList()
		},
		onReachBottom() {
			if(this.isloding)return;
			this.shopListquery.pagenum++
			this.getShopList()
		},
		onPullDownRefresh() {
			this.shopListquery.pagenum=1
			this.goodShopList=[]
			this.isloding=false
			this.ismore=false
			this.getShopList(()=>{uni.stopPullDownRefresh()})
		},
		methods:{
			async getShopList(cb){
				this.isloding=true
				const {data:res}=await uni.$http.get('/api/public/v1/goods/search',
				this.shopListquery)
				this.isloding=false
				cb&&cb()
				if(res.meta.status!==200){
					return uni.$showmsg()
				}
				this.goodShopList=[...this.goodShopList,...res.message.goods]
				if(res.message.goods.length==0){
					this.ismore=true;
					this.isloding=true
				}else{
					this.ismore=false
					this.isloding=false
				}
			},
			goShopDetails({goods_id}){
				uni.navigateTo({
					url:'/subpkg/shop_details/shop_details?id='+goods_id
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.more-no{
		padding: 20px 0;
		text-align: center;
	}
.shop-list-item{
	display: flex;
	padding: 10px;
	border-bottom: 1px solid #efefef;
	font-size: 13px;
	.left-img{
		image{
			width: 100px;
			height: 100px;
		}
	}
}
.right-text{
	display: flex;
	flex-direction: column;
	justify-content: space-between;
	padding-left: 20px;
	.right-text-bottom{
		font-size: 18px;
		color: rgb(192,0,0);
	}
}
</style>
