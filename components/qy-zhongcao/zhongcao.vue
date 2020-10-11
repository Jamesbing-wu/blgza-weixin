<template>
	<view class="fond-new">
		<block v-for="(item,index) of list" :key="index">
			<view class='fenxiang'>
				<view class='app'>
					<view class="fx_01">
						<view class="fx_01_l" v-if="item.user"><img :src="item.user.headpic"></view>
						<view class="fx_01_r" v-if="item.user">
							{{item.user.nickname}}<br /><span>{{item.create_time}}</span>
						</view>
					</view>
					<view class="fx_02">{{item.title}}</view> 
					<view class="fx_04" > 
						<li :class="is_show" :key="index" :ref="index">
							<text v-if="item.done.is_more==0">{{item.done.article}}</text>
							<text v-if="item.done.is_more==1" class="li_text" >
								{{item.done.sort_article}}</text>
							<text v-if="item.done.is_more==2">{{item.done.article}}</text>
						</li>
					</view>
					<block v-if="item.done.is_more != 0">
						<view class="xsqbhz"  v-if="item.done.is_more == 1" @click="xianshi(index)">显示全部</view>
						<view class="xsqbhz"  v-if="item.done.is_more == 2" @click="shou(index)">收起</view>
					</block> 
					<view class="fx_07" >
						<block v-for="(i,ins) of item.image" :key="ins" v-if="ins<9">
							<img :src="i" @click="bigimage(item.image,ins)">
						</block>
					</view>
					<view class="fx_08" @click="jump_pro(item.goods.id)">
						<view class="fx_08_l"><img :src="item.goods.imgs"></view>
						<view class="fx_08_r">
							<view class="fx_08_r_1">{{item.goods.goods_name}}</view>
							<view class="price-box">
								<text class="prices">¥{{item.goods.price}}</text>
							</view> 
						</view>
					</view>
				</view> 
				<view style="height:80upx"></view>
			</view>
		</block> 
			<view class="xinde" @click="jump_write"> 
				<img src="./camera.png"  />
				<view class="xd_t">发布心得</view>
			</view> 

	</view>
</template>

<script>
	import uniIcon from "@/components/uni-icon/uni-icon.vue"  
	export default {
		data() {
			return {
				is_show:'li',
				data:'',
				height: "40px",
				hidden_height: '70px',
				qw: true,
				list:[]
			}
		},
		props:{
			slist:Array
		},
		components: {
			uniIcon
		}, 
		mounted() { 			
			this.list=this.slist
			console.log('slist:',this.list)
			this.jisuan()
		},
		methods: {
			jump_write(){
				this.$emit('jump_write')
			},
			xianshi(index) {
				let state = this.list[index].done.is_more
				if (state == 1) {
					this.list[index].done.is_more = 2
					let a = this.list[index]
					this.$set(a,a.done.is_more,2)
					this.is_show = 'show'
				}
			},
			shou(index){				
				let a = this.list[index]
				a.done.is_more = 1
				this.list[index].done.sort_article = a.done.sort_article				
			},
			jisuan() {
				const list = this.list
				let arr = []
				for (let k in list) {
					const v = list[k]
					arr[k] = {}
					if (v.article.length > 60) {
						arr[k].article = v.article
						arr[k].sort_article = v.article.substr(0,60)
						arr[k].is_more = 1;
					} else {
						arr[k].article = v.article
						arr[k].sort_article = v.article
						arr[k].is_more = 0;
					}
					this.list[k].done = arr[k]
					this.data = arr[k]
				}
			},

			bigimage(imageList,ins) {
				uni.previewImage({
					current: ins,
					urls: imageList
				});
			},
			zsqw() {
				this.height = "80px",
					this.qw = false
			}, 
			 

		}
	}
</script>

<style lang='scss'>
	.li_text{line-height: 20px;overflow:hidden;text-overflow:ellipsis;white-space: normal;display:-webkit-box;-webkit-box-orient:vertical;-webkit-line-clamp:2;}
	.li {
	  list-style: none;
	  padding: 0px;
	  margin: 0px;
	}
	.xsqbhz{color: #428BCA;margin-top: 5px;}
	.fond-new {
		background-color: #F2F2F2;
		min-height: 100vh;
		padding: 1px 0;
		font-size: 14px;
		/* 小程序  */
		/* #ifdef MP-WEIXIN */
		.xinde {
			display: flex;
			background: linear-gradient(to right, #FF597A, #E02E4E);
			padding: 7px 20px;
			border-radius: 30px;
			position: fixed;
			bottom: 10px;box-sizing: border-box;
			left: 50%;
			width: 140px;
			margin-left: -70px;
			img{
				width:28px;
				height:28px;
				padding-top:3px;
			}
		}
		
		/* #endif */
		
		/* 除小程序以外程序  */
		/* #ifndef MP-WEIXIN */
		.xinde {
			display: flex;
			background: linear-gradient(to right, #FF597A, #E02E4E);
			padding: 7px 20px;
			border-radius: 30px;box-sizing: border-box;
			position: fixed;
			bottom: 30px;
			left: 50%;
			width: 140px;
			margin-left: -70px;
			img{
				width:28px;
				height:28px;
				padding-top: 3px;
			}
		}
		
		/* #endif */
		.xd_t {
			padding:5px 0 0 5px;font-size: 16px;color: #fff;
		}
		
		.fenxiang{
			padding: 10px;background-color: #fff;margin: 10px;border-radius: 8px;position: relative;
			.close{position: absolute;top: 5px; right:10px;}
			.fx_01{display: flex;}
			.fx_01_l{padding-right: 10px}
			.fx_01_l img{width: 40px;height: 40px;border-radius: 40px;}
			.fx_01_r{line-height: 20px;}
			.fx_01_r span{font-size: 12px;color: #A2A2A2;}
			.fx_02{font-weight: bold;padding:15px 0 5px; }
			.fx_03{line-height: 20px;}
			.fx_04{line-height: 20px;padding-bottom:20px;overflow: hidden;}
			.show{
				height: 100%;
				list-style: none;
				padding: 0px;
				margin: 0px;
			}
			.fx_05{color: #5F7FBA;font-size: 12px;position: absolute;left: 0;bottom: 0px;padding-top: 20px;z-index: 99;background-color:rgba(255,255,255,0.5);width: 100%;height: 20px;line-height: 20px;}
			.fx_06{height: 20px;position: relative;}
			.fx_07{padding:10px 0;overflow: hidden;box-sizing: border-box;}
			.fx_07 img{width: 26vw;margin: 5px 1.8%;height: 26vw;border-radius: 3px;}
			.fx_08{background-color: #f2f2f2;border-radius: 8px;margin: 10px 0;padding: 8px;display: flex;}
			.fx_08_l{display: flex;flex-direction: column;justify-content: center;}
			.fx_08_l img{width: 100px;height: 100px;border-radius: 8px;}
			.fx_08_r{padding-left: 10px;padding-top: 3px;line-height: 20px;display: flex;flex-direction: column;justify-content:space-between;}
			.fx_08_r_1{max-height: 40px;overflow: hidden;}
			.fx_08_r_2{font-size: 10px;}
			.fx_08_r_2 span{font-size: 18px;}
			.price-box {
				display: flex;
				align-items: baseline;
				height: 64upx;
				padding: 10upx 0;
				font-size: 12px;
				color: #fa436a;
			}
			
			.price {
				
				margin:0px 6px 0 2px;
			}
			.prices{
				margin:0px 0px 0 2px;}
			.m-price {
				margin: 0 12upx;
				color: #909399;
				text-decoration: line-through;
			}
			
			.coupon-tip {
				align-items: center;
				padding: 4upx 10upx;
				background: #fa436a;
				color: #fff;
				border-radius: 6upx;
				line-height: 1;
				transform: translateY(-4upx);
			}
			.coupon-tip-grey {
				background: #999;
				align-items: center;
				padding: 4upx 10upx;
				color: #fff;
				border-radius: 6upx;
				line-height: 1;
				transform: translateY(-4upx);
			}
		}
	}
</style>
