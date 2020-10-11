<template>
	<view class="text-box">
		<view class="text-area">
			<textarea  placeholder="输入你想说的..."  v-model="shareMessage"/>
			<view class="choose-image-area">
				<view v-if="imgList.length!==0" v-for="(img,index) in imgList" :key="index">
					<image :src="imgList[index]" mode="aspectFill">
						<view class="delete-image" @click="deleteImage(index)">删除</view>
					</image>
					
					<!-- <image class="delete-icon" src="../../static/icons/delete.png" mode="aspectFill"></image> -->
				</view>
				
			</view>
			<view class="select-photo" @click="choosePhoto">
				<image src="../../static/icons/az2.png" mode="widthFix" ></image>
				<text>选择图片</text>
			</view>
			
			<view class="button">
				<button type="default" @click="addShare">提交</button>
				<button type="warn" @click="cancleShare">取消</button>
			</view>
			
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				shareMessage:'',
				userInfo:getApp().globalData.userInfo,
				imgList:[],
				messageList:null,
				follow:0,
				userList:[]
			}
		},
		methods:{
			addShare:function(){
				var user={};
				if(this.shareMessage==''&&this.imgList.length==0){
					uni.showModal({
						title:'警告',
						content:'请输入文字或者上传图片!'
					})
				}
				else{
					user.avatarUrl=this.userInfo.avatarUrl;
					user.nickName=this.userInfo.nickName;
					user.createTime=this.getTime();
					user.article=this.shareMessage;
					user.imgurl=this.imgList;
					this.postMessageImage();
					uni.navigateBack({
						delta:1
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
			cancleShare:function(){
				uni.navigateBack({
					delta:1
				})
				
			},
			choosePhoto:function(){
				
				uni.chooseImage({
					success: (res) => {
						this.imgList=this.imgList.concat(JSON.parse(JSON.stringify(res.tempFilePaths)))
						console.log(this.imgList)
					}
					
				}
				
				)
				
			},
			deleteImage:function(index){
				this.imgList.splice(index,1);
			},
			
			postImage:function(){
				console.log('图片长度为'+this.imgList.length)
				console.log(this.follow)
				for(var i=0;i<this.imgList.length;i++)
				{
					console.log('传输'+i)
					uni.uploadFile({
						url:'https://www.blgza.com/formal/add_mespic.php',
						filePath:this.imgList[i],
						name:'image',
						header:{"Content-Type": "multipart/form-data"},
						formData:{
							follow:this.follow,
							name:'james',
							opid:getApp().globalData.openid
						},
						success:(res)=>{
							
							
						},
						complete: (res) => {
							uni.navigateBack({
								delta:1
							})
						}
					})
				}
				console.log('图片传输完毕')
				this.imgList=[];
			},
			
			postMessage: function(){
				return new Promise((resolve)=>{
					uni.request({
						url:'https://www.blgza.com/formal/add_mes.php',
						method:'POST',
						header: {
						        'content-type': 'application/x-www-form-urlencoded'
						      },
						dataType: 'json',
						responseType: 'text',
						data:{
							opid:getApp().globalData.openid,
							mes_text:this.shareMessage
						},
						success: resolve
						
						
					}
				)}
					)
				
				},
				
				
			postMessageImage:async function(){
				
				var promiseValue=await this.postMessage();
				this.follow=promiseValue.data;
				this.postImage()
				
			}
		}
	}
</script>

<style>
	.text-area{
		width: 100%;
		
	}
	.text-area textarea{
		height: 300upx;
	}
	
	.text-box{
		border: #00FFFF ;
		border-radius: 10upx;
		background-color: #d9d9d9;
		
		margin-top: 5%;
		display: flex;
		justify-content: center;
	}
	.select-photo{
		margin-left: 30upx;
	}
	.select-photo image{
		width: 75upx;
		height: 75upx;
		border-radius: 10upx;
		margin-top: 30upx;
	}
	.choose-image-area image{
		width: 180upx;
		height: 180upx;
		border-radius: 10upx;
		margin-left: 2upx;
	}
	.choose-image-area{
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
	}
	.delete-image{
		margin-left: 40upx;
		color: #FF0000;
	}
	.button{
		margin-top: 40upx;
	}
</style>
