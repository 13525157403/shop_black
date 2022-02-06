<template>
	<view>
		<shopSearch @click="goSearchPage"></shopSearch>
		<view class="cate">
			<scroll-view :scroll-y="true" class="cate-left" :style="{height: maxVH+'px'}">
				<view :class="['left-text',navctiveIndex==index?'navict':'']" 
				v-for="(item,index) in cateList" :key="index" 
				@click="navictClick(index)">
				<text>{{item.cat_name}}</text>
				</view>
			</scroll-view>
			<scroll-view :scroll-y="true" class="cate-right" 
			:style="{height: maxVH+'px'}" :scroll-top="scrollRightTop">
				<view class="" v-for="(item,index) in catechildrenList" :key="index">
					<view class="right-title">
						/{{item.cat_name}}/
					</view>
					<view class="right-shop">
						<view v-for="(i,m) in item.children" :key="m" @click="navictShop(i)">
							<image  :src="i.cat_icon" mode="" ></image>
							<text>{{i.cat_name}}</text>
						</view>
						<view></view>
						<view></view>
					</view>
				</view>
			</scroll-view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				cateList:[],
				catechildrenList:[],
				navctiveIndex:0,
				maxVH:0,
				scrollRightTop:0
			};
		},
		onLoad() {
		const sysInfo = uni.getSystemInfoSync()
		this.maxVH = sysInfo.windowHeight-50
		this.cateHttp()
		},
		methods:{
		async cateHttp(){
				const {data:res}=await uni.$http.get('/api/public/v1/categories')
				if (res.meta.status !== 200) return uni.$showmsg();
				this.cateList = res.message
				this.catechildrenList=res.message[0].children
			},
			navictClick(index){
				this.scrollRightTop=Math.random()
				this.navctiveIndex=index
				this.catechildrenList=this.cateList[index].children
			},
			navictShop({cat_id}){
				uni.navigateTo({
					url:'/subpkg/shop_list/shop_list?id='+cat_id
				})
			},
			goSearchPage(){
				uni.navigateTo({
					url:'/subpkg/shop_search/shop_search'
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.cate{
		display: flex;
		font-size: 12px;
	}
	.cate-left{
		width: 240rpx;
		font-size: 12px;
		background-color: rgb(247,247,247);
	}
	.left-text{
		padding: 50rpx 0;
		text{
			width: 100%;
			padding: 10rpx 0;
			padding-left: 40rpx;
			text-align: center;
		}
	}
	.navict{
		background-color: rgb(255,255,255);
		text{
			border-left: 8rpx solid rgb(192,0,0);
		}
	}
	.cate-right{
		
	}
	.right-title{
		width: 100%;
		text-align: center;
		padding: 15rpx 0;
	}
	.right-shop{
		
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
	}
	.right-shop>view{
		
		width: 30%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		image{
			width: 120rpx;
			height: 120rpx;
		}
	}
</style>
