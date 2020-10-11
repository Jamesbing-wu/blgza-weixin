<template>
	<view class="container">
		<swiper :indicator-dots="indicatorDots" :autoplay="autoPlay" >
			<swiper-item v-for="(img,index) in swiperItemUrls" :key="index" class="swiper-itemA">
				<image :src="img" mode="widthFix" @click="BigImageSwiper(swiperItemUrls,index)" ></image>
			</swiper-item>
		
		</swiper>
		<view class="content">
			<uni-collapse accordion="true">
			    <uni-collapse-item title="原产品分类" style="text-align: center;">
			        <uni-list>
			            <uni-list-item title="帽子" @click="jumpToHat" >	</uni-list-item>
			            <uni-list-item title="裙子" @click="jumpToDress"></uni-list-item>
			            <uni-list-item title="包" @click="jumpToBag"></uni-list-item>
						<uni-list-item title="发卡" @click="jumpToPin"></uni-list-item>
						<uni-list-item title="领带" @click="jumpToTie"></uni-list-item>
						<uni-list-item title="衬衫" @click="jumpToShirt"></uni-list-item>
						<uni-list-item title="围巾" @click="jumpToScarf"></uni-list-item>
						<uni-list-item title="外套" @click="jumpToOutcoat"></uni-list-item>
						<uni-list-item title="马甲" @click="jumpToWaistcoat"></uni-list-item>
						
			        </uni-list>
			    </uni-collapse-item>
			</uni-collapse>
		</view> 
		<!-- <view class="title">
			原产品
		</view> -->
		<view class="product-list">
			<view v-if="ImageList0" v-for="(img,index) in ImageList0" :key='test' class="product" @click="localJumpto(index)">
				<view >
					<image v-if="img.data.exh_url" class="image-wrapper" :src="img.data.exh_url" mode="aspectFill" ></image>
				</view>
				<text v-if="img.data.exh_text" class="product-title" >{{img.data.exh_text}}</text>
			</view>
		</view>
		<!-- <view class="title">
			文创产品
		</view> -->
		<view class="content">
			<uni-collapse accordion="true">
			    <uni-collapse-item title="文创产品分类" style="text-align: center;">
			        <uni-list>
			            <uni-list-item title="手机壳" @click="jumpToPhone" >	</uni-list-item>
			            <uni-list-item title="鞋子" @click="jumpToShoes"></uni-list-item>
			            <uni-list-item title="书包" @click="jumpToBag1"></uni-list-item>
						<uni-list-item title="短袖" @click="jumpToShort"></uni-list-item>
						<uni-list-item title="长袖" @click="jumpToLong"></uni-list-item>
						<uni-list-item title="衬衫" @click="jumpToShirt1"></uni-list-item>
						<uni-list-item title="裙子" @click="jumpToDress1"></uni-list-item>
						<uni-list-item title="卫衣" @click="jumpToSweater"></uni-list-item>
						<uni-list-item title="情侣服" @click="jumpToCouple"></uni-list-item>
						
			        </uni-list>
			    </uni-collapse-item>
			</uni-collapse>
		</view>
		 <view class="product-list">
		 	<view v-if="ImageList1" v-for="(img,index) in ImageList1" :key='test' class="product" @click="localJumpto_1(index)">
		 		<view >
		 			<image v-if="img.data.exh_url" class="image-wrapper" :src="img.data.exh_url" mode="aspectFill" ></image>
		 		</view>
		 		<text v-if="img.data.exh_text" class="product-title" >{{img.data.exh_text}}</text>
		 	</view>
		 </view>
	</view>
</template>

<script>
	import uniCollapse from '@/components/uni-collapse/uni-collapse.vue'
	import uniCollapseItem from '@/components/uni-collapse-item/uni-collapse-item.vue'
	import uniIcons from "@/components/uni-icons/uni-icons.vue"
	export default {
		components: {uniCollapse,uniCollapseItem,uniIcons},
		
		data() {
			return {
				indicatorDots:true,
				autoPlay:true,
				
				swiperItemUrls:{
					
					},
				
				ImageList0:[],
				ImageList1:[]
			}
		},
		
		onReady:function(){
			uni.request({
				url:'https://www.blgza.com/formal/get_exh.php',
				method:'POST',
				dataType:'json',
				header:{
					'content-type':'application/x-www-form-urlencoded;charset=utf-8'
				},
				data:{
					belong:0
				},
				success: (res) => {
					this.ImageList0=res.data
					
				}
			})
			uni.request({
				url:'https://www.blgza.com/formal/get_exh.php',
				method:'POST',
				dataType:'json',
				header:{
					'content-type':'application/x-www-form-urlencoded;charset=utf-8'
				},
				data:{
					belong:1
				},
				success: (res) => {
					this.ImageList1=res.data
					
				}
			})
			uni.request({
				url:'https://www.blgza.com/formal/get_banner.php',
				method:'POST',
				dataType:'json',
				header:{
					'content-type':'application/json;charset=utf-8'
				},
				success: (res) => {
					console.log(res.data.data);
					this.swiperItemUrls=res.data.data;
				}
			})
			
		},
		methods: {
			
			BigImage:function(ImageList,index){
				var urlList=[];
				for(let i=0;i<ImageList.length;i++)
				{
					urlList.push(ImageList[i].data.exh_url);
				}
				uni.previewImage({
					current:index,
					urls:urlList
				})
			},
			
			BigImageSwiper:function(ImageList,index){
				uni.previewImage({
					current:index,
					urls:ImageList
				})
			},
			
			jumpToHat:function(){
				uni.navigateTo({
					url:"../goods/yuanchuang/hat"
				})
			},
			jumpToBag:function(){
				uni.navigateTo({
					url:"../goods/yuanchuang/bag"
				})
			},
			jumpToShirt:function(){
				uni.navigateTo({
					url:"../goods/yuanchuang/shirt"
				})
			},
			jumpToPin:function(){
				uni.navigateTo({
					url:"../goods/yuanchuang/pin"
				})
			},
			jumpToTie:function(){
				uni.navigateTo({
					url:"../goods/yuanchuang/Tie"
				})
			},
			jumpToOutcoat:function(){
				uni.navigateTo({
					url:"../goods/yuanchuang/outcoat"
				})
			},
			jumpToScarf:function(){
				uni.navigateTo({
					url:"../goods/yuanchuang/scarf"
				})
			},
			jumpToWaistcoat:function(){
				uni.navigateTo({
					url:"../goods/yuanchuang/waistcoat"
				})
			},
			jumpToDress:function(){
				uni.navigateTo({
					url:"../goods/yuanchuang/dress"
				})
			},
			localJumpto:function(index){
				var functions=[this.jumpToHat,this.jumpToBag,this.jumpToPin,this.jumpToScarf,this.jumpToTie,this.jumpToDress,this.jumpToShirt,this.jumpToOutcoat,this.jumpToWaistcoat];
				console.log(index)
				
				return functions[index]();
			},
			jumpToBag1:function(){
				uni.navigateTo({
					url:"../goods/wenchuang/bag"
				})
			},
			jumpToCouple:function(){
				uni.navigateTo({
					url:"../goods/wenchuang/couple"
				})
			},
			jumpToDress1:function(){
				uni.navigateTo({
					url:"../goods/wenchuang/dress"
				})
			},
			jumpToLong:function(){
				uni.navigateTo({
					url:"../goods/wenchuang/long"
				})
			},
			jumpToPhone:function(){
				uni.navigateTo({
					url:"../goods/wenchuang/phone"
				})
			},
			jumpToShirt1:function(){
				uni.navigateTo({
					url:"../goods/wenchuang/shirt"
				})
			},
			jumpToShoes:function(){
				uni.navigateTo({
					url:"../goods/wenchuang/shoes"
				})
			},
			jumpToShort:function(){
				uni.navigateTo({
					url:"../goods/wenchuang/short"
				})
			},
			jumpToSweater:function(){
				uni.navigateTo({
					url:"../goods/wenchuang/sweater"
				})
			},
			localJumpto_1:function(index){
				var functions=[this.jumpToShoes,this.jumpToPhone,this.jumpToBag1,this.jumpToShort,this.jumpToLong,this.jumpToSweater,this.jumpToCouple,this.jumpToShirt1,this.jumpToDress1];
				console.log(index)
				
				return functions[index]();
			},
		},
		onLoad:function(){
			
			// if(!getApp().globalData.userInfo){
			// 	uni.showModal({
			// 		title:'警告',
			// 		content:'尚未进行授权，请点击确定跳转到授权页面进行授权。',
			// 		success:function(res){
			// 			if(res.confirm){
			// 				uni.navigateTo({
			// 					url:"../login/loginButtton"
			// 				})
			// 			}
			// 		}
			// 	})
			// }
			var opid=uni.getStorageSync('opid')
			
			uni.checkSession({
				success: () => {
					console.log('当前session_key有效')
					getApp().globalData.openid=opid
					uni.request({
						url:'https://www.blgza.com/formal/get_userinf.php',
						method:'POST',
						header: {
						        'content-type': 'application/x-www-form-urlencoded'
						      },
						data:{
							opid:opid
						},
						success: (res) => {
							console.log(res.data)
							var userinfo=new Object()
							userinfo.nickName=res.data.data['nickName']
							userinfo.avatarUrl=res.data.data['avatarUrl']
							getApp().globalData.phone=res.data.data['pho_num']
							getApp().globalData.userInfo=userinfo
							
							console.log(getApp().globalData.userInfo)
							if(!getApp().globalData.userInfo){
								uni.navigateTo({
									url:'../login/loginButtton'
								})
							}
						}
					})
					console.log('请求成功')
					
				},
				fail: () => {
					console.log('session失效')
					// uni.navigateTo({
					// 	url:'../login/loginButtton'
					// })
				},
				complete: () => {
					console.log('完成')
					console.log(getApp().globalData.userInfo)
					
				}
			})
			
		}
	}
</script>

<style>
	.container {
		
	}
	
	
	swiper{
		height: 250upx;
		display: flex;
	}
	
	swiper image{
		width: 750upx;
		border-radius: 10upx;
	}
	.title{
		text-align: center;
		margin-top: 40upx;
		margin-bottom: 20upx;
		background-color: #00AAFF;
		line-height: 80upx;
		font-weight: bold;
	}
	
	.product-list{
		display: flex;
		justify-content: center;
		flex-wrap: wrap;
		text-align: center;
	}
	.image-wrapper{
		width: 200upx;
		height: 200upx;
		margin-left: 20upx;
		margin-right: 20upx;
		margin-top: 20upx;
		margin-bottom: 10upx;
		border-radius: 10upx;
		
		
	}
	.product-title{
		
		margin-bottom: 40upx;
		font-size: 40upx;
		/* text-shadow: 0 0 2px black; */
		
	}
	.product-title:hover{
		
		margin-bottom: 40upx;
		font-size: 40upx;
		text-shadow: 0 0 2px red;
	}
	
	uni-collapse-item{
		background-color: #00aaff;
		color: #000000;
		font-weight: bold;
		
	}
	.product{
		margin-bottom: 20upx;
	}
</style>
