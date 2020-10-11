<template>
	<view class="container">
		<view class="Liuyan-title">
			<text class="title">留言信息</text>
			<view class="scroll">
				<scroll-view scroll-y="true" scroll-top="0" show-scrollbar="true" >
					<view class="liuyan-item" v-for="(item,index) in liuyan_list" :key="index">
						<text>{{item.content}}</text>
						
						<view class="img-group">
							<view v-if="item.img_list" v-for="(img,indexImage) in item.img_list" :key="indexImage">
								<image :src="img" @click="BigImage(index,indexImage)" mode="aspectFill" class="image"></image>
							</view>
						</view>
						<view style="align-self: flex-end;">
							<text style="width: 37%; font-size: 70%;color: #646566; margin-right: 30upx;">{{item.time}}
							</text>
							<image class="deleteIcon" src="../../static/icons/delete.png" mode="widthFix" @click="deleteItem(index)"></image>
						</view>
					</view>
				</scroll-view>
				
			</view>
		</view>
		
		<!-- <view class="line"></view>   
		
		<view class="Liuyan-title add">
			<text class="title">添加留言</text>
			<view class="add-liuyan">
				<textarea v-model="liuyan" placeholder="请输入留言..."></textarea>
			</view>
		</view>
		<view class="add-btn">
			<button type="default" @click="addLiuyan">提交留言</button>
		</view> -->
	</view>
</template>

<script>
	export default{
		data(){
			return{
				liuyan_list:[
					
				],
				liuyan:''
			}
		},
		onLoad:function(){
			// this.getMyMessage()
		},
		onShow:function(){
			console.log('页面刷新')
			this.getMoreMessage()
		},
		
		methods:{
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
			deleteItem:function(index){
				console.log(this.liuyan_list[index].content+':'+this.liuyan_list[index].id)
				uni.showModal({
					title:'提醒',
					content:'是否删除此留言？',
					
					success: (res) => {
						if(res.confirm)
						{
							uni.request({
								url:'https://www.blgza.com/formal/del_mes.php',
								method:'POST',
								header:{
									'Content-type':'application/x-www-form-urlencoded;charset=utf-8'
								},
								data:{
									id:this.liuyan_list[index].id
								}
							})
							
							this.liuyan_list.splice(index,1)
						}
					}
				})
			},
			
			BigImage:function(index,indexImage){
				uni.previewImage({
					urls:this.liuyan_list[index].img_list,
					current:indexImage
				})
			},
			// addLiuyan:function(){
			// 	var liuyan=new Object();
				
				
			// 	if(this.liuyan.length===0){
			// 		uni.showModal({
			// 			title:'警告',
			// 			content:'留言内容不能为空'
						
			// 		})
			// 	}
			// 	else{
			// 		liuyan.content=this.liuyan;
			// 		liuyan.time=this.getTime();
			// 		uni.showModal({
			// 			title:'提示',
			// 			content:'确认提交？',
			// 			success: (res) => {
			// 				if(res.confirm){
			// 					this.liuyan_list.push(liuyan);
			// 					this.liuyan='';
								
			// 				}
			// 			}
			// 		})
			// 	}
			// },
			getMyMessage:function(){
				uni.request({
					url:'https://www.blgza.com/formal/get_mymes.php',
					method:'POST',
					
					header:{
						'Content-type':'application/x-www-form-urlencoded;charset=utf-8'
					},
					data:{
						opid:getApp().globalData.openid
					},
					success: (res) => {
						var json=JSON.stringify(res.data)
						var obj=JSON.parse(json)
						for(var i=0;i<obj.length;i++)
						{
							var data=obj[i].data;
							var liuyan=new Object();
							liuyan.content=data.message;
							liuyan.time=data.time;
							liuyan.img_list=data.picture
							this.liuyan_list.push(liuyan)
							
						}
					}
				})
			},
			
			getMoreMessage:function(){
				uni.request({
					url:'https://www.blgza.com/formal/get_mymes.php',
					method:'POST',
					
					header:{
						'Content-type':'application/x-www-form-urlencoded;charset=utf-8'
					},
					data:{
						opid:getApp().globalData.openid
					},
					success: (res) => {
						console.log(res.data)
						this.liuyan_list=[];
						var json=JSON.stringify(res.data)
						var obj=JSON.parse(json)
						for(var i=0;i<obj.length;i++)
						{
							var data=obj[i].data;
							var liuyan=new Object();
							liuyan.content=data.message;
							liuyan.time=data.time;
							liuyan.img_list=data.picture
							liuyan.id=data.id
							if(liuyan in this.liuyan_list)
							{
								
							}else{
								this.liuyan_list.push(liuyan);
								console.log('新增一条')
							}
							
						}
					}
				})
			}
		}
		
		
		
	}
</script>

<style>
	.container{
		display: flex;
		flex-direction: column;
	}
	
	.Liuyan-title{
		margin-top: 5%;
		margin-left: 5%;
		margin-right: 5%;
	}
	.title{
		width: 70%;
		background-color: #FF5500;
		border-radius: 10upx;
		line-height: 80upx;
		font-size: 50upx;
		color: #FFFFFF;
		text-align: center;
	}
	.add-btn{

	}
	.liuyan-area{
		height: 500upx;
	}
	.liuyan-item{
		margin-top: 50upx;
		margin-bottom: 50upx;
		background-color: #e5e5e5;
		display: flex;
		border-radius: 10upx;
		flex-direction: column;
	}
	
	.liuyan-item text{
		width: 650upx;
	}
	/* .delete-icon{
		width: 50upx;
		height: 50upx;
		align-self: flex-end;
		background-color: #FF0000;
		border-radius: 50%;
	} */
	.add-liuyan{
		background-color: #D3D3D3;
		border: solid;
	}
	.add-liuyan textarea{
		align-self: center;
	}
	.line{
		border-top: solid;
		height: 1upx;
		text-align: center;
		margin-left: 5%;
		margin-right: 5%;
		border-radius: 20upx;
		border-color: #CCCCCC;
	}
	.image{
		width: 210upx;
		height: 210upx;
		
		margin-right: 5upx;
		margin-left: 5upx;
		border-radius: 10upx;
	}
	.img-group{
		display: flex;
		flex-wrap: wrap;
		margin-top: 100upx;
	}
	.deleteIcon{
		width: 50upx;
		height: 50upx;
		margin-right: 30upx;
		background-color: #FF0000;
		border-radius: 50%;
		
	}
</style>
