<template>
	<view class="goods-bag">
		<view v-if="bagsList" v-for="(bag,index) in bagsList" :key='index' class="product">
			<view class="image-wrapper">
				<image v-if="bag.exh_url" :src="bag.exh_url" mode="widthFix" @click="BigImage(index)"></image>
				
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				bagsList:[]
			}
		},
		methods:{
			BigImage:function(index){
				var urlList=[];
				for(let i=0;i<this.bagsList.length;i++)
				{
					urlList.push(this.bagsList[i].exh_url);
				}
				uni.previewImage({
					current:index,
					urls:urlList
				})
			}
		},
		onLoad:function(){
			uni.request({
				url:'https://www.blgza.com/formal/get_byc.php',
				method:'POST',
				data:{
					class:'shoes'
				},
				dataType:'json',
				header:{
					'content-type':'application/x-www-form-urlencoded'
				},
				success: (res) => {
					var list=res.data;
					for(let i=0;i<res.data.length;i++)
					{
						this.bagsList.push(res.data[i].data)
					}
				}
			})
		}
	}
	
</script>

<style>
	.goods-bag{
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
	}
	image{
		width: 300upx;
		height: 300upx;
		margin-left: 20upx;
		margin-right: 20upx;
		margin-top: 20upx;
		margin-bottom: 20upx;
		background:#f3f3f3;
		border-radius: 20upx;
		border-style: groove;
	}
	.title{
		margin-left: 20upx;
		margin-right: 20upx;
		margin-top: 20upx;
		margin-bottom: 20upx;
		font-size: 40upx;
		text-align: center;
	}
	
</style>