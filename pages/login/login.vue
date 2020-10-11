<template>
	<view class="container">
		<view class="input-group">
			<view class="input-row">
				<text class="title">账号：</text>
				<m-input class="m-input" type="text" clearable focus v-model="username" placeholder="请输入账号"></m-input>
			</view>
			<view class="input-row">
				<text class="title" style="padding-left: 20upx;margin-left: 25upx;">密码：</text>
				<m-input class="m-input" type="password" displayable v-model="password" placeholder="请输入密码"></m-input>
			</view>
			
		</view>
		<view class="login-btn">
			<button type="primary" @tap="test">登陆</button>
		</view>
		<view class="register">
			<navigator url="./register">注册账号</navigator>
		</view>
	</view>
</template>

<script>
	import mInput from '../../components/m-input.vue'
	export default{
		components:{
			mInput
		},
		data(){
			return{
				showPassword:true
			}
		},
		
		methods:{
			test:function(){
				console.log('success')
			}
			}
		
	}
</script>

<style>
	.container{
		display: flex;
		flex-direction: column;
		padding: 50upx;
		align-items: center;
		background-color: #d9d9d9;
	}
	.input-group{
		margin-top: 100upx;
	}
	.input-row{
		border: solid 2upx #555555;
		border-radius: 10upx;
		line-height: 100upx;
		display: flex;
		justify-content: center;
		font-size: 35upx;
		width: 100%;
		background-color: #FFFFFF;
		
	}
	.title{
		
		}
	.login-btn{
		width: 80%;
		
		margin-top: 50upx;
		}
		
	.register{
		margin-top: 25upx;
		color: #007AFF;
	}
	
		
</style>
