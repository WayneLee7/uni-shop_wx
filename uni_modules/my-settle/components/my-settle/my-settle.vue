<template>
	<view class="my-settle-container">
		<!-- 全选 -->
		<label class="radio" @click="changeAllState">
			<radio value="" color="#C00000" :checked="isFullCheck"/><text>全选</text>
		</label>
		<!-- 合计 -->
		<view class="amount-box">
			合计：<text class="amount">¥{{checkGoodsAmount}}</text>
		</view>
		<!-- 解散 -->
		<view class="btn-settle" @click="settlement">
			结算({{checkedCount}})
		</view>
	</view>
</template>
<script>
	import {mapGetters,mapMutations} from 'vuex'
	export default{
		computed:{
			...mapGetters('m_cart',['checkedCount','total','checkGoodsAmount']),
			...mapGetters('m_user',['addStr']),
			isFullCheck(){
				return this.checkedCount==this.total
			}
		},
		methods:{
			...mapMutations('m_cart',['updateAllGoodsState']),
			...mapMutations('m_user',['updateRedirectInfo']),
			changeAllState(){
				this.updateAllGoodsState(!this.isFullCheck)
			},
			settlement(){
				if(!this.checkedCount) return uni.$showMsg('请选择要结算的商品')
				if(!this.addStr) return uni.$showMsg('请选择收货地址')
				if(!this.token) return this.delayNavigate()
			},
			showTips(n){
				uni.showToast({
					title:'请登录后结算，'+n+'秒之后自动跳转到登录页面',
					icon:'none',
					mask:true,
					duration:1500
				})
			},
			delayNavigate(){
				this.seconds=3
				this.showTips(this.seconds)
				this.timer=setInterval(()=>{
					this.seconds--
					if(this.seconds<=0){
						clearInterval(this.timer)
						uni.switchTab({
							url:'/pages/my/my',
							success:()=> {
								this.updateRedirectInfo({
									openType:'switchTab',
									from:'/pages/cart/cart'
								})
							}
						})
						return
					}
					this.showTips(this.seconds)
				},1000)
			}
		},
		data(){
			return{
				seconds:3,
				timer:null
			}
		}
	}
</script>
<style lang="scss">
	.my-settle-container{
		position: fixed;
		bottom: 0;
		left: 0;
		width: 100%;
		height: 50px;
		background-color: white; 
		display: flex;
		justify-content: space-between;
		align-items: center;
		font-size: 14px;
		padding-left: 5px;
		.radio{
			display: flex;
			align-items: center;
			text{
				
			}
		}
		.amount-box{
			.amount{
				color: #c00000;
				font-weight: bold;
			}
		}
		.btn-settle{
			background-color: #c00000;
			height: 50px;
			color: white;
			line-height: 50px;
			padding:  0 10px;
			min-width: 100px;
			text-align: center;
		}
	}
</style>
