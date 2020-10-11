<template>
	<view class="xinde" >
		
		<view class="title">
			<view class="tit">
				<view class="title_l"><input v-model="form.title" placeholder="标题"/></view>
				<view class="title_r">0/30</view>
			</view>
		</view>
		<view class="xd">
			<textarea v-model="form.article"  placeholder="分享你的使用心得..."></textarea>
		</view>
		<view class="pic">
			<robby-image-upload :value="pics" :server-url="serverUrl" :form-data="formData" :server-url-delete-image="DelServelUrl" 
			@delete="delImage" @add="onImg" @move="onMove" :limit="3"></robby-image-upload>
		</view>
		
		<view class="tuij" v-if="!produ">
			<uni-icon type="plusempty" size="30" color="#CBCBC9"></uni-icon>
			<view class="tj_t">添加推荐商品</view>
		</view>
		
		<view class="product" v-if="produ">
			<view class="pro_l"><img :src="zc.imgs"></img></view>
			<view class="pro_r">
				<view class="pro_r_01">{{zc.goods_name}}</view>
				<view class="pro_r_02"><span>¥</span>{{zc.price}}</view>
			</view>
			<view class="close" @click="close_pro"><uni-icon type="minus-filled" size="23" color="#CBCBC9"></uni-icon></view>
		</view> 
		
		
		<button class="add-btn" @click="sub()">提交发布</button>
	</view>
</template>

<script>
	import robbyImageUpload from '@/components/plan-image-upload/up_img'
	import uniIcon from "@/components/uni-icon/uni-icon.vue" 
	export default {
		data() {
			return {
				produ:false,
				zc:{},
				serverUrl:'zc_up_img',
				DelServelUrl: 'del_img',
				formData: {
					use: 'zc',
					back: 'idurl'
				},					
				pics: [],
				photos: [],
				form:{
					title:'',
					article:'',
					goods_id:'', 
				}
			};
		},
		components: {
			uniIcon,
			robbyImageUpload
		},
		mounted() {
			const zc=uni.getStorageSync('zc')
			if(zc){
				this.zc=zc
				this.form.goods_id=zc.id
				this.produ=true
			}
		},
		methods:{
			close_pro(){
				this.produ=false
				uni.removeStorageSync('zc')
			},
			onImg(e) {
				this.pics = e.allImages
				const obj = e.allImages
				for (let index in obj) {
					let id = obj[index].id
					this.photos[index] = id
				}
			},
			sub(){ 
				if(this.form.title.length<3 || this.form.article.length<3 || this.form.goods_id==''){
					uni.showToast({
						title: '请填写标题并选择商品',
						icon:'none'
					});
					return;
				}
				this.form.imgs=this.photos 
			}
		}
	}
</script>

<style lang="scss">
.xinde{font-size: 14px; background-color: #F7F7F5;min-height: 100vh;
.add-btn{
		position: fixed;
		left: 30upx;
		right: 30upx;
		bottom: 16upx;
		z-index: 95;
		display: flex;
		align-items: center;
		justify-content: center;
		width: 690upx;
		height: 80upx;
		font-size: 32upx;
		color: #fff;
		background-color: #fa436a;
		border-radius: 10upx;
		box-shadow: 1px 2px 5px rgba(219, 63, 96, 0.4);		
	}
	.pic{  
		background-color: #fff;
	}
	.title{background-color: #fff; padding: 10px;
		.tit{display: flex; justify-content: space-between;padding: 10px 0;  border-bottom: 1px solid #F6F6F4;line-height: 20px;
			.title_l{font-size: 16px;width: 80%;}
			.title_r{color: #A0A0A0;}
		}
	}
	.xd{background-color: #fff;
		textarea{padding: 10px 20px 20px;width: 100%;line-height: 20px;}
	}
	.tuij{border-radius: 5px;;margin: 10px;border: 1px #E3E3E3 dashed;background-color: #FDFDFB;display: flex;height: 80px;
	justify-content: center; align-items: center;
		.tj_t{color: #CCCCCA;font-size: 16px;}
	}
	.product{border-radius: 5px;margin: 15px 10px;background-color: #FDFDFB;display: flex;height: 80px; position: relative;
		.pro_l{width: 80px;
			img{width: 80px;height: 80px;}
		} 
		.pro_r{padding: 10px;font-size: 16px; width: 80%;
			.pro_r_01{height: 20px;line-height: 20px;overflow: hidden;white-space: nowrap;text-overflow: ellipsis;}
			.pro_r_02{color: #DA3056; font-size: 16px; padding-top: 15px; font-weight: 600;
				span{font-size: 12px;}
			}
		}
		.close{position: absolute;top:-10px;right: -9px;}
	}
	.ruhe{padding: 10px;text-align: center;color: #999997;font-size: 15px;}
}
</style>
