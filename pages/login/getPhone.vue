<template>
	<view>
		<view style="position: relative; text-align: center;">
			<image src="http://jamesbing.cn/image/%E5%9B%BE%E7%89%873.png" mode="aspectFit"></image>
			
			<!-- <text class="text">您尚未登录，请点击下方按钮进行授权登录！</text> -->
		</view>
		<view class="line"></view>
		<view style="display: flex; flex-direction: column; align-items: flex-start;">
			<text class="h1">授权后开发者将获取一些权限</text>
			<text class="h2">● 获取你的手机号</text>
			
		</view>
		
		<button open-type="getPhoneNumber" @getphonenumber="getPhoneNumber" type="primary">获取手机号</button>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				
			}
		},
		methods:{
			getPhoneNumber:function(e){
				var encryptData=e.detail.encryptedData;
				console.log(e.detail)
				var iv=e.detail.iv;
				
					uni.request({
						url:'https://www.blgza.com/formal/phone.php',
						method:'POST',
						header: {
						        'content-type': 'application/x-www-form-urlencoded'
						      },
						dataType: 'json',
						responseType: 'text',
						data:{
							openid:getApp().globalData.openid,
							appid:'wx43c7a28a03203079',
							session_key:getApp().globalData.sessionKey,
							data:encryptData,
							iv:iv,
							code:getApp().globalData.code
						},
						success: (res) => {
							var infoObject=res.data;
							getApp().globalData.phone=infoObject.phoneNumber;
							getApp().globalData.signin=1;
							console.log(res.data)
						}
					})
				
				uni.navigateBack({
					delta:1
				})
			}
			}
	}
</script>

<style>
	button{
		
		width: 84%;
		margin-top: 60upx;
	}
	
	image{
		/* margin-top: 60upx; */
		margin-top: 30upx;
		margin-bottom: 40upx;
	}
	.text{
		position: absolute;
		z-index: 2;
		left: 10upx;
		top: 100upx;
		
		
		
		
	}
	.h1{
		font-size: 45upx;
		font-weight: bold;
		margin-left: 10%;
	}
	.h2{
		color: #999999;
		font-size: 35upx;
		margin-left: 10%;
		margin-top: 20upx;
	}
	.line{
		border-bottom: solid;
		border-color: #cecece;
		margin-bottom: 30upx;
		width: 90%;
		margin-left: 5%;
	}
</style>
