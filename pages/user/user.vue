<template>
	<view class="container">
		<view class="user-section">
			<image class="bg" src="../../static/user-bg.jpg" mode="widthFix"></image>
			<view class="user-info-box">
				<view class="portrait-box">
					<image class="protrait" :src="userInfo.avatarUrl||'../../static/missing-face.png'"></image>
				</view>
				<view class="info-box">
					<text class="username" @click="login">{{userInfo.nickName || '点击授权登录'}}</text>
					
				</view>
			</view>
		</view>
		<view class="uni-list">
			<uni-list>
				<uni-list-item title="我的留言" thumb="../../static/icons/2.png" @click="jumpToLiuyan"></uni-list-item>
				<!-- <uni-list-item title="消息" thumb="../../static/icons/3.png"> -->
					
				</uni-list-item>
				
				<uni-list-item title="关于我们" thumb="../../static/icons/az1.png" @click="jumpToAbout"></uni-list-item>
				<uni-list-item title="联系客服" thumb="../../static/icons/azg.png" @click="jumpToKefu"></uni-list-item>
				<!-- <uni-list-item title="登陆" @click="login"></uni-list-item> -->
			</uni-list>
		</view>
	</view>
</template>

<script>
	import uniIcons from "@/components/uni-icons/uni-icons.vue"
	export default{
		
		components:{uniIcons},
		data(){
			return {
				userInfo:getApp().globalData.userInfo,
				
			}
		},
		methods:{
			jumpToAbout:function(){
				uni.navigateTo({
					url:"../userInfo/aboutUs"
				})
			},
			jumpToLiuyan:function(){
				if(getApp().globalData.phone===null){
					uni.showModal({
						title:'警告',
						content:'您尚未授权登陆，登陆后可查看您的留言。',
					})
				}
				else{
					uni.navigateTo({
						url:"../userInfo/liuyan"
					})
				}
			},
			jumpToKefu:function(){
				uni.navigateTo({
					url:"../userInfo/kefu"
				})
			},
			
			login:function(){
				if(getApp().globalData.signin===0 && !getApp().globalData.userInfo){
					uni.showModal({
						title:'提醒',
						content:'您尚未登录，是否登录',
						confirmText:'确定登录',
						cancelText:'取消登录',
						success: (res) => {
							if(res.confirm)
							uni.navigateTo({
								url:'../login/loginButtton'
							})
						}
					})
				}
			}
		},
		
		
		onLoad:function(){
			if(getApp().globalData.phone===null && getApp().globalData.userInfo){
				uni.showModal({
					title:'注意',
					content:'检测到您的手机号尚未注册,点击确定前往注册手机号。',
					cancelText:'取消注册',
					success: (res) => {
						if(res.confirm){
							uni.navigateTo({
								url:"../login/getPhone"
							})
						}
					}
				})
			}},
		onShow:function(){
			this.userInfo=getApp().globalData.userInfo
			console.log('更新用户信息')
			
		}	
				
					
			
		
		
	}
</script>

<style>
	.container{
		display: flex;
		flex-direction: column;
		
	}
	.username{
		font-size: 50upx;
		
	}
	
	.user-info-box{
		display: flex;
		position: relative;
		top: 150upx;
		left: 50upx;
	}
	.bg{
		height: 400upx;
		opacity: 0.5;
		position: absolute;
		object-fit: cover;
		width: 750upx;
	}
	.protrait{
		border-radius: 50%;
		width: 150upx;
		height: 150upx;
		position: relative;
		border: solid #FFFFFF 8upx;
	}
	.info-box{
		position: relative;
		top:60upx;
		left: 20upx;
		display: flex;
		flex-direction: column;
	}
	.uni-list{
		position: relative;
		top: 100upx;
	}
	
</style>
