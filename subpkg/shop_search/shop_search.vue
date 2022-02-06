<template>
	<view>
		<view class="search">
			<uni-search-bar :focus="showFocus" :radius="100" @confirm="search" placeholder="请输入搜索内容" cancelButton="none"
				v-model="searchValue" bgColor="#fff" @input="searchInput"></uni-search-bar>
		</view>
		<view class="search-list" v-if="searchValue">
			<view class="shop-item" v-for="(item,index) in searchList" 
			:key="index" @click="goShopDetails(item)">
				<text>{{item.goods_name}}</text>
				<uni-icons type="right"></uni-icons>
			</view>
		</view>
		<view class="search-history" v-else>
			<view class="search-history-title">
				<text>搜索历史</text>
				<uni-icons type="trash" size="18" @click="clearHistoryList"></uni-icons>
			</view>
			<view class="search-history-tag">
				<view v-for="(item,index) in searHistorys" :key="index" 
				@click="goShopList(item)">
					{{item}}
				</view>
			</view>

		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				searHistoryList: [],
				searchList: [],
				searchValue: '',
				timer: null,
				showFocus:false
			};
		},
		computed:{
			searHistorys(){
				return [...this.searHistoryList].reverse()
			}
		},
		onLoad() {
			this.showFocus=true
		this.searHistoryList=JSON.parse(uni.getStorageSync('searHistory'))||[]	
		},
		methods: {
			searchInput(e) {
				clearTimeout(this.timer)
				this.timer=setTimeout(() => {
					this.searchHttp()
				}, 800)
			},
			async searchHttp() {
				if (this.searchValue == '') return;
				const {data: res} = await uni.$http.get('/api/public/v1/goods/search', 
				{query: this.searchValue})
				if (res.meta.status !== 200) {
					return uni.$showmsg()
				}
				this.searchList = res.message.goods
				this.saveSearchHistory()
			},
			saveSearchHistory(){
				if (this.searchValue == '') return;
				const setArr=new Set(this.searHistoryList)
				setArr.delete(this.searchValue)
				setArr.add(this.searchValue)
				this.searHistoryList=[...setArr]
				uni.setStorageSync('searHistory',JSON.stringify(this.searHistoryList))
			},
			clearHistoryList(){
				this.searHistoryList=[]
				uni.setStorageSync('searHistory',[])
			},
			goShopList(query){
				uni.navigateTo({
					url:'../shop_list/shop_list?query='+query
				})
			},
			goShopDetails({goods_id}){
				uni.navigateTo({
					url:'../shop_details/shop_details?id='+goods_id
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.search {
		height: 50px;
		background-color: rgb(216, 30, 6);
	}

	.shop-item {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 12px 5px;
		border-bottom: 1px solid #efefef;
		font-size: 13px;

		text {
			// 文字不允许换行（单行文本）
			white-space: nowrap;
			// 溢出部分隐藏
			overflow: hidden;
			// 文本溢出后，使用 ... 代替
			text-overflow: ellipsis;
			margin-right: 3px;
		}
	}

	.search-list,
	.search-history {
		padding-left: 5px;
	}

	.search-history-title {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 10px 5px;
		border-bottom: 1px solid #efefef;

		text {
			font-size: 13px;
		}

	}

	.search-history-tag {
		display: flex;
		flex-wrap: wrap;
		font-size: 12px;
	}

	.search-history-tag>view {
		padding: 3px;
		text-align: center;
		min-width: 30px;
		background-color: rgb(243, 245, 247);
		margin: 5px 5px 0 0;
	}
</style>
