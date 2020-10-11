<template>
	<view class="main">
		<web-view src="https://mp.weixin.qq.com/s/N35JQuWEbxEw7ekM2CA5GQ"></web-view>
		
	</view>
</template>

<script>
	export default{
		data(){
			return{
				imageUrl:'http://jamesbing.cn/image/194b542adbbbac8885f737df08295e3.jpg'
			}
		},
		methods:{
			BigImage:function(){
				var list=[];
				list.push(this.imageUrl)
				uni.previewImage({
					urls:list,
					current:0
				})
			}
		}
	}
</script>

<style>
	.main{
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	.title{
		margin-top: 40upx;
		font-size: 60upx;
		font-weight: bold;
		line-height: 60upx;
		text-align: center;
	}
	.image{
		margin-top: 100upx;
		
	}
</style>
