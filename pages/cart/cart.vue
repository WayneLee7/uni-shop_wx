<template>
	<view class="cart-container" v-if="cart.length!==0">
		<my-address></my-address>
		<view class="cart-title">
			<uni-icons type="shop" size="18"></uni-icons>
			<text class="cart-title-text">购物车</text>
		</view>

		<uni-swipe-action>
			<block v-for="(good,i) in cart" :key="i">
				<uni-swipe-action-item :right-options="option" @click="swiperItemClickHandle(good)">
					<my-goods :goods="good" showRadio @radio-change="radioChange" :showNum="true"
						@num-change="numChange"></my-goods>
				</uni-swipe-action-item>
			</block>

		</uni-swipe-action>
		
		<!-- 结算 -->
		<my-settle></my-settle>
	</view>
	
	
	<view class="empty-cart" v-else>
		<text>空空如也</text>
	</view>
</template>

<script>
	import badgeMix from '@/mixins/tabbar-badge.js'
	import {mapState,mapMutations} from 'vuex'
	export default {
		mixins: [badgeMix],

		computed: {
			...mapState('m_cart', ['cart'])
		},
		data() {
			return {
			option:[{
				text:'删除',
				style:{
					backgroundColor:'#C00000'
				}
			}]
			};
		},
		onShow() {

		},
		methods: {
			...mapMutations('m_cart', ['updateGoodsState', 'updateGoodsCount','removeGoodsById']),
			radioChange(e) {
				this.updateGoodsState(e)
			},
			numChange(e) {
				// console.log(e);
				this.updateGoodsCount(e)
			},
			swiperItemClickHandle(good){
				this.removeGoodsById(good.goods_id)
			}
		}
	}
</script>

<style lang="scss">
	.cart-container{
		padding-bottom: 50px;
	}
	.cart-title {
		height: 40px;
		display: flex;
		align-items: center;
		font-style: 14px;
		padding-left: 5px;
		border-bottom: 1px solid #efefef;

		.cart-title-text {
			font-size: 14px;
			margin-left: 10px;
		}
	}
	.empty-cart{
		display: flex;
		justify-content: center;
		text{
			
		}
	}
</style>
