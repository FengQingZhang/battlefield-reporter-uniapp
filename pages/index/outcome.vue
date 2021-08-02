<template>
	<view class="content">
		<view class="top-bg" style="background: url(https://trackercdn.com/cdn/tracker.gg/bfv/bfv-profile-hero1.jpg);background-size:cover;background-position: 50%;">
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
							<view style="color: #FFFFFF;margin-left: 1rpx;">{{username}}</view>
						</view>
						<view class="user-info-row" style="height: 20%;">
							<view>
								<van-tag mark type="success">等级:{{level}}</van-tag>
							</view>
							<view style="margin-left:5%;">
								<van-tag mark type="warning">时长:{{timePlayed|timePlayed_filter}}</van-tag>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<van-tabs ref="vantabs" active="a" nav-class="my_nav" title-inactive-color="#FFFFFF" title-active-color="#FFFFFF" >
		  <van-tab title="总览" name="a">
			  <van-grid square>
			    <van-grid-item :key='index' use-slot v-for="(item,index) in overview">
					<ul>
						<li>{{item.label}}</li>
						<li>{{item.value}}</li>
					</ul>
				</van-grid-item>
			  </van-grid>
		  </van-tab>
		  <van-tab title="枪械" name="b">内容 2</van-tab>
		  <van-tab title="载具" name="c">内容 3</van-tab>
		  <van-tab title="对局" name="d">内容 4</van-tab>
		</van-tabs>
	</view>
</template>

<script>
	import url from '../../common/config.js'
	export default {
		data() {
			return {
				head_src: '',
				username: '',
				level:'',
				timePlayed:'',
				platformSlug: '',
				overview:[],
				
			}
		},
		methods: {
			getInfoByName() {
				uni.request({
					url: url.EA.find_standings_url + this.platformSlug + "/" + this.username,
					success: (res) => {
						let data = res.data.data.segments[0].stats;
						this.level = data.rank.value;
						this.timePlayed = data.timePlayed.displayValue;
						this.overview.push({'label':'SPM','value':data.scorePerMinute.displayValue});
						this.overview.push({'label':'KPM','value':data.killsPerMinute.displayValue});
						this.overview.push({'label':'K/D','value':data.kdRatio.displayValue});
						this.overview.push({'label':'命中率','value':data.shotsAccuracy.displayValue});
						this.overview.push({'label':'胜率','value':data.wlPercentage.displayValue});
						this.overview.push({'label':'击杀数','value':data.kills.displayValue});
						this.overview.push({'label':'死亡数','value':data.deaths.displayValue});
						this.overview.push({'label':'爆头数','value':data.headshots.displayValue});
						let  num = (data.headshots.value/data.kills.value)*100;
						this.overview.push({'label':'爆头率','value':num.toPrecision(3)+"%"});
						this.overview.push({'label':'胜利场次','value':data.wins.displayValue});
						let count = data.wins.vlaue+data.losses.value;
						let kill = (data.kills.vlaue/count)*100;
						let death = (data.deaths.value/count)*100;
						this.overview.push({'label':'场均击杀','value':kill.toPrecision(3)+"%"});
						this.overview.push({'label':'场均死亡','value':death.toPrecision(3)+"%"});
						this.overview.push({'label':'最远爆头','value':data.longestHeadshot.displayValue});
						this.overview.push({'label':'最高连杀','value':data.killStreak.displayValue});
						this.overview.push({'label':'失败场次','value':data.losses.displayValue});
						console.log(data);
					},
					fail: () => {
						console.log("请求失败");
					}
				})
			},
		},
		filters:{
			timePlayed_filter(val){
				if(val.indexOf("h")!=-1){
					let temp = val.substring(0,val.indexOf("h"))+"小时";
					return temp;
				}else if(val.indexOf("m")!=-1){
					let temp = val.substring(0,val.indexOf("m"))+"分钟";
					return temp;
				}else{
					return val;
				}
			}
		},
		onLoad(option) {
			this.head_src = option.avatarUrl;
			this.username = option.username;
			this.platformSlug = option.platformSlug;
			this.getInfoByName();
			this.$refs['vantabs'].resize();
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
		height: 40%;
		padding-left: 5%;
		display: flex;
		flex-direction: row;
		align-items: center;
	}
	.content_div{
		height:86%;
		width: 100%;
	}
	.my_nav{
		background-color: #364e80;
	}
	.view-content{
		width: 100%;
		height: 100%;
		background-color: #111b2b;
	}
</style>
