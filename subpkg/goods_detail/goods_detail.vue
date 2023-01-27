<template>
	<view v-if="goods_info.goods_name" class="goods_detail_container">
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
			<swiper-item v-for="(item,i) in goods_info.pics" :key="i">
				<image :src="item.pics_big" @click="preview(i)"></image>
			</swiper-item>
		</swiper>
		
		<view class="goods-info-box">
			<view class="price">
				{{goods_info.goods_price}}
			</view>
			<view class="goods-info-body">
				<view class="goods-name">
					{{goods_info.goods_name}}
				</view>
				<view class="favi">
					<uni-icons type="star" size="18" color="gray"></uni-icons>
					<text>收藏</text>
				</view>
			</view>
			<view class="yf">
				快递：免运费
			</view>
		</view>
		
		<rich-text :nodes="goods_info.goods_introduce"></rich-text>
		
		<!-- 下导航区域 -->
		 <view class="goods_nav">
		 	<uni-goods-nav :fill="true" :options="options" :button-group="buttonGroup" @click="onClick" @buttonClick="buttonClick" />
		 </view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				goods_info:{},
				options:[
					{
						text:'店铺',
						icon:'shop'
					},
					{
						text:'购物车',
						icon:'cart',
						info:2
					}
				],
				buttonGroup:[
					{
						text:'加入购物车',
						backgroundColor:'#ff0000',
						color:'#fff'
					},
					{
						text:'立即购买',
						backgroundColor:'#ffa200',
						color:'#fff'
					}
				]
			}
		},
		methods: {
			async getGoodsDetails(goods_id){
				const {data:res}=await  uni.$http.get('/api/public/v1/goods/detail',{goods_id})
				if(res.meta.status!==200) return uni.$showMsg()
				res.message.goods_introduce=res.message.goods_introduce.replace(/<img /g,'<img style="display:block;" ')
				this.goods_info=res.message
				
			},
			preview(i){
				console.log(i);
				uni.previewImage({
					urls:this.goods_info.pics.map(item=>item.pics_big_url),
					current:i
				})
			},
			onClick(e){
				if(e.content.text==='购物车'){
					uni.switchTab({
						url:'/pages/cart/cart'
					})
				}
			},
			buttonClick(){
				
			}
		},
		onLoad(option) {
			const goods_id=option.goods_id
			this.getGoodsDetails(goods_id)
		}
	}
</script>

<style lang="scss">
swiper{
	height: 750rpx;
	image{
		width: 100%;
		height: 100%;
	}
}
.goods-info-box{
	padding: 10px;
	padding-right: 0;
	.price{
		color: #c00000;
		font-size: 18px;
		margin: 10px 0;
	}
	.goods-info-body{
		display: flex;
		justify-content: space-between;
		.goods-name{
			font-style: 13px;
			margin-right: 10px;
		}
		.favi{
			width: 120px;
			font-size: 12px;
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: center;
			border-left: 1px solid #efefef;
			color: gray;
		}
}
.yf{
	font-size: 12px;
	color: gray;
	margin: 10px 0;
}
}
.goods_nav{
	position: fixed;
	left: 0;
	bottom: 0;
	width: 100%;
}
.goods_detail_container{
	padding-bottom: 50px;
}
</style>
