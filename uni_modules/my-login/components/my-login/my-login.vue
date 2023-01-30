<template>
	<view class="login-container">
		<uni-icons type="contact-filled" size="100" color="#AFAFAF"></uni-icons>
		    <button type="primary" class="btn-login" @click="getUserProfile">一键登录</button>
		<view class="tips-text">
			登录后尽享更多权益
		</view>
	</view>
</template>
<script>
	import {mapMutations,mapState} from 'vuex'
	export default{
		data(){
			return{
				
			}
		},
		computed:{
			...mapState('m_user',['redirectInfo'])
		},
		methods:{
			...mapMutations('m_user',['updateUserInfo','updateToken','updateRedirectInfo']),
			   getUserProfile() {
			        uni.getUserProfile({
			          desc: '你的授权信息',
			          success: (res) => {
			            // 将信息存到 vuex 中
						// console.log(res);
			            this.updateUserInfo(res.userInfo)
			            this.getToken(res)
			          },
			          fail: (res) => {
			            return uni.$showMsg('您取消了登录授权')
			          }
			        })
		},
		async getToken(info){
			// const [err,res] = await uni.login().catch(err=>err)
			// if(err||res.errMsg!=='login:ok') return uni.$showError('登录失败')
			// const query={
			// 	code:res.code,
			// 	encryptedData:info.encryptedData,
			// 	iv:info.iv,
			// 	rawData:info.rawData,
			// 	signature:info.signature
			// }
			// const {data:loginResult}=await uni.$http.post('/api/public/v1/users/wxlogin',query)
			// console.log(loginResult);
			// if(loginResult.meta.status!==200) return uni.$showMsg('登录失败！')
			uni.$showMsg('登陆成功')
			this.updateToken('dasd321gh3j3h2h21')
			console.log(this.redirectInfo);
			if(this.redirectInfo&&this.redirectInfo.openType=='switchTab'){
				uni.switchTab({
					url:this.redirectInfo.from,
					complete:()=>{
						this.updateRedirectInfo({})
					}
				})
			}
			
		}
	}
	}
</script>
<style lang="scss">
	.login-container{
		height: 750rpx;
		background-color: #F8F8F8;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		.btn-login{
			width: 90%;
			border-radius: 100px;
			margin: 15px;
			background-color: #C00000;
		}
		.tips-text{
			font-size: 12px;
			color: gray;
		}
	}
</style>
