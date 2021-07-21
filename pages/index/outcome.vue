<template>
	<view class="content">
		<view class="top-div">
			<view class=""></view>
			<van-image width="15vw" height="15vw" fit="contain" :src="head_src" radius="5px" />
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				head_src:'',
				username:'',
			}
		},
		methods: {

		},
		onLoad(option){
			this.head_src = option.avatarUrl;
			this.username = option.username;
		}
	}
</script>
<style>
	.content {
		height: 100%;
		width: 100%;
	}
	.top-div {
		height: 15%;
		width: 80%;
		margin: 0 auto;
	}
	.head-img{
		width: ;
	}
</style>
