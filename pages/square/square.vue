<template>
	<view class="container">
		
		<view class="comment-item">
			<view v-for="(user,userNum) in userList" :key="user" style="display: flex; flex-direction: column; align-content: center;">
				<view class="user-section">
					<image v-if="user.avatarUrl" class="avatar" :src="user.avatarUrl" mode="widthFix"></image>
					<view class="nickName" v-if="user.nickName" style="margin-left: 10upx;">
						<text>{{user.nickName}}</text>
						<view>
							<view class="article" v-if="user.article">
								<text>{{user.article}}</text>
							</view>
							<view v-if="user.imgurl"  style="display: flex;flex-wrap: wrap; margin-top: 100upx;">
								<view v-for="(image,index) in user.imgurl" :key="index">
									<image class="image-show" :src="image" mode="aspectFill" @click="BigImage(user.imgurl,index,userNum)"></image>
									
								</view>
								
							</view>
							<view class="createTime" v-if="user.createTime">
								<text >{{user.createTime}}</text>
							</view>
						</view>
					</view>
					
				</view>
				
				<view class="line"></view>
			</view>
		
		</view>
		
		
		<view class="requirement" @click="jumpWrite">
			<image src="../../static/camera.png" ></image>
			<text class="requirement-text">发表留言</text>
			
		</view>
		
	</view>
</template>

<script>
	
	export default{
		
		data(){
			return{
				userList:[],
				
				shareMessage:'',
				userInfo:getApp().globalData.userInfo,
				imgList:[],
				messageList:null,
				follow:0
				
			}
		},
		onShow:function(){
			this.getMessage()
			console.log('页面回显')
		},
		onBackPress:function(){
			
		},
		
		
		methods:{
			BigImage:function(urls,index,userNum){
				var urllist=[]
				for(let i=0;i<urls.length;i++)
				{
					urllist.push(urls[i])
				}
				console.log(urls.length,index)
				uni.previewImage({
					current:index,
					// urls:this.userList[userNum].imgurl
					urls:urllist
				})
			},
			jumpWrite:function(){
				this.userInfo=getApp().globalData.userInfo;
				console.log(this.userInfo)
				if(!getApp().globalData.userInfo)
				{
					uni.showModal({
						title:'警告',
						content:'用户尚未进行授权，无法发表留言。点击确定跳转到授权页面进行授权。',
						confirmText:'确认授权',
						cancelText:'取消授权',
						success:function(res){
							if(res.confirm){
								uni.navigateTo({
									url:"../login/loginButtton"
								})
							}
						}
					})
				}
				else if(getApp().globalData.phone===null){
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
					}
				
				else{
					
					uni.navigateTo({
						url:"./addShare"
					})
				}
			},
			
			getTime:function(){
				var date = new Date(),
				year = date.getFullYear(),
				month = date.getMonth() + 1,
				day = date.getDate(),
				hour = date.getHours() < 10 ? "0" + date.getHours() : date.getHours(),
				minute = date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes(),
				second = date.getSeconds() < 10 ? "0" + date.getSeconds() : date.getSeconds();
				month >= 1 && month <= 9 ? (month = "0" + month) : "";
				day >= 0 && day <= 9 ? (day = "0" + day) : "";
				var timer = year + '/' + month + '/' + day + ' ' + hour + ':' + minute + ':' + second;
				return timer;
			},
			
			
			
			getMessage:function(){
				uni.request({
					url:'https://www.blgza.com/formal/get_mes.php',
					method:'POST',
					dataType:'json',
					header:{
						'content-type':'application/json;charset=utf-8'
					},
					success: (res) => {
						this.userList=[]
						var json=JSON.stringify(res.data)
						var obj=JSON.parse(json)
						for(var i=0;i<obj.length;i++){
							var data=obj[i].data;
							var obj_item=new Object();
							obj_item.avatarUrl=data.port;
							obj_item.nickName=data.name;
							obj_item.createTime=data.time;
							obj_item.article=data.mes_text;
							obj_item.imgurl=data.pic_url;
							this.userList.push(obj_item)
							
						}
						
						

					},
					fail: (res) => {
						console.log(res.message)
					}
				})
			}

		}
		
	}
	
</script>

<style>
	.avatar{
		width: 100upx;
		height: 100upx;
		border-radius: 50%;
		border: solid #FFFFFF;
		
	}
	.container{
		
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	.comment-item{
		width: 700upx;
	}
	.user-section{
		display: flex;
		margin-top: 100upx;
		
		
	}
	.nickName{
		align-self: center;
		
		font-size: 40upx;
		margin-top: 20upx;
	}
	.createTime{
		
		font-size: 30upx;
		margin-top: 50upx;
		color: #3B4144;
		
	}
	.article{
		margin-top: 50upx;
		
	}
	
	.image-show{
		width: 200upx;
		height: 200upx;
		margin-right: 10upx;
		
		border-radius: 10upx;
	}
	.requirement{
		background-color: #00aaff;
		position: fixed;
		bottom: 30upx;
		display: flex;
		justify-content: center;
		border-radius: 30upx;
	}
	.requirement image{
		width: 70upx;
		height:70upx;
		padding: 20upx;
	}
	.requirement-text{
		font-size: 50upx;
		padding: 20upx;
		margin-left: -10upx;
	}
	
	.line{
		border-top: solid;
		height: 1upx;
		text-align: center;
		margin-left: 1%;
		margin-right: 1%;
		border-radius: 20upx;
		border-color: #CCCCCC;
		margin-top: 100upx;
	}
</style>
