<template>
	<view class="goods-item">
		<view class="goods-item-left">
			<radio :checked="goods.goods_state" color="#C00000" v-if="showRadio" @click="radioClickHandle"></radio>
			<image :src="goods.goods_small_logo||defaultPic" class="goods-pic"></image>
		</view>
		
		<!-- 右侧 -->
		<view class="goods-item-right">
			<view class="goods-name">
				{{goods.goods_name}}
			</view>
			<view class="goods-info-box">
				<view class="goods-price">
					¥{{goods.goods_price|tofixed}}
				</view>
				<uni-number-box :min="1" :value="goods.goods_count" v-if="showNum" @change="numChangeHandle"></uni-number-box>
			</view>
		</view>
	</view>
</template>
<script>
	export default{
		data(){
			return {
				
			}
		},
		props:{
			goods:{
				type:Object,
				default:{}
			},
			showRadio:{
				type:Boolean,
				default:false
			},
			showNum:{
				type:Boolean,
				default:false
			}
		},
		methods:{
			radioClickHandle(){
				this.$emit('radio-change',{
					goods_id:this.goods.goods_id,
					goods_state:!this.goods.goods_state
				})
			},
			numChangeHandle(val){
				this.$emit('num-change',{
					goods_id:this.goods.goods_id,
					goods_count:+val
				})
			}
		},
		onLoad(){
			
		},
		filters:{
			tofixed(num){
				return Number(num).toFixed(2)
			}
		}
	}
</script>
<style lang="scss">
	.goods-item{
		width: 750rpx;
		box-sizing: border-box;
		display: flex;
		padding: 10px 5px;
		border-bottom: 1px solid #f0f0f0;
		.goods-item-left{
			margin-right: 5px;
			display: flex;
			align-items: center;
			justify-content: space-between;
			.goods-pic{
				width: 100px;
				height: 100px;
				display: block;
			}
		}
		.goods-item-right{
			display:flex;
			flex-direction: column;
			justify-content: space-between;
			.goods-name{
				font-size: 13px;
			}
			.goods-info-box{
				display: flex;
				align-items: center;
				justify-content: space-between;
				.goods-price{
					color:#C00000;
					font-size: 16px;
				}
			}
		}
	}
</style>
