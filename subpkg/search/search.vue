<template>
	<view>
		<view class="search-box">
			<uni-search-bar @input="input" :radius="100" cancelButton="none" :focus="true"/>
		</view>
		
		<view class="sugg-list" v-if="searchResults.length!==0">
			<view class="sugg-item" v-for="(item,i) in searchResults" :key="i" @click="gotoDetail(item.goods_id)">
				<view class="goods_name">
					{{item.goods_name}}
				</view>
				<uni-icons type="arrowright" size="16"></uni-icons>
			</view>
		</view>
		
		<!-- 搜索历史 -->
		<view class="history-box" v-else>
			<view class="history-title">
				<text>搜索历史</text>
				<uni-icons type="trash" size="17" @click="clean"></uni-icons>
			</view>
			<view class="history-list">
				<uni-tag :text="item" v-for="(item,i) in historys" :key="i" @click="gotoGoodsList(item)"></uni-tag>
			</view>
		</view>
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				timer:null,
				kw:'',
				searchResults:[],
				historyList:[]
			};
		},
		onLoad() {
		this.historyList=JSON.parse(uni.getStorageSync('kw')||'[]')	
		},
		methods:{
			input(e){
				clearTimeout(this.timer)
				this.timer=setTimeout(()=>{
					this.kw=e
					this.getSearchList()
				},500)
			},
			async getSearchList(){
				if(this.kw===''){
					this.searchResults=[]
					return
				}
			const {data:res} = await uni.$http.get('/api/public/v1/goods/qsearch?query='+this.kw)
			if(res.meta.status!==200) return uni.$showMsg()
			this.searchResults=res.message	
			this.saveSearchHistory()
			},
			gotoDetail(goods_id){
				uni.navigateTo({
					url:'/subpkg/goods_detail/goods_detail?goods_id='+goods_id
				})
			},
			saveSearchHistory(){
				// this.historyList.push(this.kw)
			const arr=	new Set(this.historyList)
			arr.delete(this.kw)
			arr.add(this.kw)
			this.historyList=Array.from(arr)
			uni.setStorageSync('kw',JSON.stringify(this.historyList))
			},
			clean(){
				this.historyList=[]
				uni.setStorageSync('kw','[]')
			},
			gotoGoodsList(kw){
				console.log("!");
				uni.navigateTo({
					url:'/subpkg/goods_list/goods_list?query='+kw
				})
			}
		},
		computed:{
			historys(){
				return [...this.historyList].reverse()
			}
		}
	}
</script>

<style lang="scss">
	.search-box{
		position: sticky;
		top: 0;
		z-index: 999;
	}
	.sugg-list{
	padding: 0 5px;
		.sugg-item{
				display: flex;
				align-items: center;
				justify-content: space-around;
				font-size: 12px;
				padding: 13px 0;
				border-bottom: 1px solid #efefef;
		}
		.goods_name{
			white-space: nowrap;
			overflow: hidden;
			text-overflow: ellipsis;
		}
	}
	.history-box{
		padding: 0 5px;
		.history-title{
			display: flex;
			justify-content: space-between;
			height: 40px;
			align-items: center;
			font-size: 13px;
			border-bottom:1px solid #efefef;
		}
		.history-list{
			display: flex;
			flex-wrap: wrap;
			.uni-tag{
				margin-top: 5px;
				margin-right: 5px;
				display: block;
			}
		}
	}
</style>
