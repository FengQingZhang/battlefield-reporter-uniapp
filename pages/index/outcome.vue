<template>
	<view class="content">
		<view class="top-bg" style="background: url(/static/outcome/standings.png);background-size:cover;background-position: 50%;">
			<view class="top-bg-mask">
				<view class="top-div">
					<view class="head-img">
						<van-image width="15vw" height="15vw" fit="contain" :src="head_src" round custom-class="head-img-border" />
					</view>
					<view class="user-info-div">
						<view class="user-info-row">
							<view>
								<icon v-if="platformSlug=='origin'" class="iconfont icon-origin tab-icon"></icon>
								<icon v-else-if="platformSlug=='xbl'" class="iconfont icon-xbox tab-icon"></icon>
								<icon v-else-if="platformSlug=='psn'" class="iconfont icon-PS tab-icon"></icon>
							</view>
							<view style="color: #FFFFFF;">{{username}}</view>
						</view>
						<br>
						<view class="user-info-row">
							<view>
								
							</view>
							<view></view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import url from '../../common/config.js'
	export default {
		data() {
			return {
				head_src: '',
				username: '',
				platformSlug: '',
			}
		},
		methods: {
			getInfoByName() {
				uni.request({
					url: url.EA.find_standings_url + this.platformSlug + "/" + this.username,
					success: (res) => {
						let temp = res.data;
						console.log(temp.data.segments[0]);
					},
					fail: () => {
						console.log("请求失败");
					}
				})
			},
		},
		onLoad(option) {
			this.head_src = option.avatarUrl;
			this.username = option.username;
			this.platformSlug = option.platformSlug;
			this.getInfoByName();
		}
	}
</script>
<style>
	.content {
		height: 100%;
		width: 100%;
	}

	.top-div {
		height: 100%;
		width: 80%;
		margin: 0 auto;
		display: flex;
		flex-direction: row;
	}

	.top-bg {
		height: 14%;
		width: 100%;
	}
	.top-bg-mask{
		background: rgba(0, 0, 0, .4);
		width: 100%;
		height: 100%;
	}
	.head-img {
		width: 20%;
		height: 100%;
	}
	.head-img-border{
		border-style: solid;
		border-color: #F0F0F0;
		border-width: 2px;
		margin-top:40rpx;
	}
	.tab-icon {
		font-size: 5vw;
		color: #FFFFFF;
	}
	.user-info-div{
		width:80%;
		height: 85%;
		margin-top: 5%;
		display: flex;
		flex-direction: column;
	}
	.user-info-row{
		width: 100%;
		height: 50%;
		padding-left: 5%;
		display: flex;
		flex-direction: row;
		align-items: center;
	}
</style>
