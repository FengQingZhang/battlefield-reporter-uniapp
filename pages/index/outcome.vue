<template>
	<view class="content">
		<aui-loading ref="aui-loading"
			:show="auiLoading.show"
			:type="auiLoading.type"
			:direction="auiLoading.direction"
			:msg="auiLoading.msg"
			:mask="auiLoading.mask">
			</aui-loading>
		<view class="top-bg"
			style="background: url(https://trackercdn.com/cdn/tracker.gg/bfv/bfv-profile-hero1.jpg);background-size:cover;background-position: 50%;">
			<view class="mask">
				<view class="top-div">
					<view class="head-img">
						<van-image width="15vw" height="15vw" fit="contain" :src="head_src" round
							custom-class="head-img-border" />
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
						</view>
					</view>
				</view>
			</view>
		</view>
		<van-tabs ref="vantabs" active="a" nav-class="my_nav" title-inactive-color="#FFFFFF"
			title-active-color="#FFFFFF">
			<van-tab title="总览" name="a">
				<view class="timeplayed-div">
					<view style="width: 10%;">
						<icon class="iconfont icon-youxi tab-icon" style="font-size: 10vw;"></icon>
					</view>
					<view class="timeplayed-text-div" style="width: 80%;margin-left:3%;">
						<view style="color: #FFFFFF;">游戏时长</view>
						<view style="color: #78b5e2;">{{timePlayed|timePlayed_filter}}</view>
					</view>
				</view>
				<van-grid square column-num="5">
					<van-grid-item :key='index' use-slot v-for="(item,index) in overview">
						<ul>
							<li style="color: #78b5e2;">{{item.label}}</li>
							<li style="color: #efefef;font-size: 1.25rem;white-space: nowrap;">{{item.value}}</li>
						</ul>
					</van-grid-item>
				</van-grid>
				<view class="top-pawn-div"
					style="background-image:url(https://trackercdn.com/cdn/tracker.gg/bfv/top-class-bg.jpg);background-size: 100% 100%;"
					>
					<view class="mask">
<!-- 						<van-skeleton title row="3" :loading="loading">
 -->						<view style="width: 100%;display: flex;flex-direction:row;">
							<view style="width: 10%;">
								<icon class="iconfont icon-jinpai" style="font-size: 10vw;color: #F0E68C;"></icon>
							</view>
							<view style="width: 85%;color: #efefef;font-size: large;padding-top: 2%;">
								最佳兵种
							</view>
						</view>
						<view style="width:100%;display: flex;flex-direction: row;">
							<view style="width: 40%;display: flex;flex-direction: column;text-align:center;">
								<van-image width="6rem" height="6rem" fit="contain" :src="topArm.metadata.imageUrl" />
								<van-row>
									<view style="color:#efefef;font-size: large;">
										<span v-text="topArm.metadata.name"></span>
									</view>
								</van-row>
								<van-row>
									<view style="color: hsla(0,0%,100%,.5);font-size: small;">
										{{topArm.stats.timePlayed.displayValue}}
									</view>
								</van-row>
							</view>
							<view style="width: 60%;text-align:center;">
								<view style="margin-top: 10%;">
								<van-row gutter="20">
									<van-col span="10">
										<ul>
											<li style="color: hsla(0,0%,100%,.5);font-size: large;"><span>兵种等级</span></li>
											<li style="color: #efefef;font-size: x-large;white-space: nowrap;">
												{{topArm.stats.rank.displayValue}}</li>
										</ul>
									</van-col>
									<van-col span="10">
										<ul>
											<li style="color: hsla(0,0%,100%,.5);font-size: large;">SPM</li>
											<li style="color: #efefef;font-size: x-large;white-space: nowrap;">
												{{topArm.stats.scorePerMinute.displayValue}}</li>
										</ul>
									</van-col>
								</van-row>
								<van-row gutter="20">
									<van-col span="10">
										<ul>
											<li style="color: hsla(0,0%,100%,.5);font-size: large;">{{topArm.stats.kdRatio.displayName}}</li>
											<li style="color: #efefef;font-size:x-large;white-space: nowrap;">
												{{topArm.stats.kdRatio.displayValue}}</li>
										</ul>
									</van-col>
									<van-col span="10">
										<ul>
											<li style="color: hsla(0,0%,100%,.5);font-size: large;">KPM</li>
											<li style="color: #efefef;font-size:x-large;white-space: nowrap;">
												{{topArm.stats.killsPerMinute.displayValue}}</li>
										</ul>
									</van-col>
								</van-row>
								</view>
							</view>
						</view>
						<!-- </van-skeleton> -->			
					</view>
				</view>
				<uni-collapse accordion style="background-color: #1b2640;">
					<uni-collapse-item v-for="(item,index) in armScore " :key='index' :title="item.metadata.name" 
					:thumb="item.metadata.imageUrl" style="background-color: #1b2640;color: #FFFFFF;">
					<view class="content" style="background-color: #1B2640;color: #FFFFFF;">
						<van-row gutter="5">
							<van-col span="10">
								使用时间:{{item.stats.timePlayed.displayValue}}
							</van-col>
							<val-col span="10">
								得分:{{item.stats.score.displayValue}}
							</val-col>
						</van-row>	
					</view>
					</uni-collapse-item>
				</uni-collapse>
			</van-tab>
			<van-tab title="枪械" name="b">内容 2</van-tab>
			<van-tab title="载具" name="c">内容 3</van-tab>
			<van-tab title="对局" name="d">内容 4</van-tab>
		</van-tabs>
	</view>
</template>

<script>
	import auiLoading from '@/components/aui-loading/aui-loading.vue';
	import url from '../../common/config.js'
	export default {
		components:{
			auiLoading
		},
		data() {
			return {
				head_src: '',
				username: '',
				level: '',
				timePlayed: '',
				platformSlug: '',
				overview: [],
				armScore: [],
				topArm: '',
				auiLoading:{
					show: false,
					type:4,
					direction:'col',
					msg:'加载中',
					mask:false,
				}
			}
		},
		methods: {
			getInfoByName() {
				uni.request({
					url: url.EA.find_standings_url + this.platformSlug + "/" + this.username,
					success: (res) => {
						let data = res.data.data.segments[0].stats;
						let num = (data.headshots.value / data.kills.value) * 100;
						let headshots_rate = num.toPrecision(3) + '%';
						let count = data.wins.value + data.losses.value;
						let kill = data.kills.value / count;
						let kill_rate = kill.toPrecision(3) + '%';
						let death = data.deaths.value / count;
						let death_rate = death.toPrecision(3) + '%';
						let segments = res.data.data.segments;
						for (let i = 0; i < segments.length; i++) {
							if (segments[i].type == "class") {
								if(segments[i].metadata.name=='Assault'){
									segments[i].metadata.name='偷鸡兵'
								}else if(segments[i].metadata.name=="Medic"){
									segments[i].metadata.name='兽医'
								}else if(segments[i].metadata.name=="Support"){
									segments[i].metadata.name='支援兵'
								}else if(segments[i].metadata.name=='Recon'){
									segments[i].metadata.name='斟茶兵'
								}else if(segments[i].metadata.name=='Pilot'){
									segments[i].metadata.name='沙扣分行员'
								}else if(segments[i].metadata.name=='Tanker'){
									segments[i].metadata.name='沙扣坦克驾驶员'
								}
								this.armScore.push(segments[i]);
							}
						}
						this.getTopArm();
						this.level = data.rank.value;
						this.timePlayed = data.timePlayed.displayValue;
						this.overview.push({
							'label': 'SPM',
							'value': data.scorePerMinute.displayValue
						});
						this.overview.push({
							'label': 'KPM',
							'value': data.killsPerMinute.displayValue
						});
						this.overview.push({
							'label': 'K/D',
							'value': data.kdRatio.displayValue
						});
						this.overview.push({
							'label': '命中率',
							'value': data.shotsAccuracy.displayValue
						});
						this.overview.push({
							'label': '胜率',
							'value': data.wlPercentage.displayValue
						});
						this.overview.push({
							'label': '击杀数',
							'value': data.kills.displayValue
						});
						this.overview.push({
							'label': '死亡数',
							'value': data.deaths.displayValue
						});
						this.overview.push({
							'label': '爆头数',
							'value': data.headshots.displayValue
						});
						this.overview.push({
							'label': '爆头率',
							'value': headshots_rate
						});
						this.overview.push({
							'label': '胜利场次',
							'value': data.wins.displayValue
						});
						this.overview.push({
							'label': '场均击杀',
							'value': kill_rate
						});
						this.overview.push({
							'label': '场均死亡',
							'value': death_rate
						});
						let longes = parseInt(data.longestHeadshot.value);
						this.overview.push({
							'label': '最远爆头',
							'value': longes
						});
						this.overview.push({
							'label': '最高连杀',
							'value': data.killStreak.displayValue
						});
						this.overview.push({
							'label': '失败场次',
							'value': data.losses.displayValue
						});
						
					},
					fail: () => {
						console.log("请求失败");
					}
				})
			},
			//通过数据得出最佳兵种
			getTopArm() {
				this.armScore;
				let score = 0;
				let temp;
				for (let i = 0; i < this.armScore.length; i++) {
					console.log(this.armScore[i]);
					if (score < this.armScore[i].stats.score.value) {
						score = this.armScore[i].stats.score.value;
						temp = this.armScore[i];
					}
				}
				this.topArm = temp;
				this.$refs['aui-loading'].hide();
			}
		},
		filters: {
			timePlayed_filter(val) {
				let temp = val;
				if (val.indexOf("h") != -1) {
					temp = val.replace("h", "小时")
				}
				if (val.indexOf("m") != -1) {
					temp = val.replace("m", "分钟")
				}
				return temp;
			}
		},
		onLoad(option) {
			this.$refs['aui-loading'].show();
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
		height: auto;
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

	.mask {
		background: rgba(0, 0, 0, .4);
		width: 100%;
		height: 100%;
	}

	.head-img {
		width: 20%;
		height: 100%;
	}

	.head-img-border {
		border-style: solid;
		border-color: #F0F0F0;
		border-width: 2px;
		margin-top: 40rpx;
	}

	.tab-icon {
		font-size: 5vw;
		color: #FFFFFF;
	}

	.user-info-div {
		width: 80%;
		height: 85%;
		margin-top: 5%;
		display: flex;
		flex-direction: column;
	}

	.user-info-row {
		width: 100%;
		height: 40%;
		padding-left: 5%;
		display: flex;
		flex-direction: row;
		align-items: center;
	}

	.content_div {
		height: 86%;
		width: 100%;
	}

	.my_nav {
		background-color: #364e80;
	}

	.view-content {
		width: 100%;
		height: 100%;
		background-color: #111b2b;
	}

	.grid-content {
		background-color: #364e80;
	}

	.timeplayed-div {
		width: 100%;
		height: 10%;
		background-color: #1b2640;
		display: flex;
		flex-direction: row;
	}

	.timeplayed-text-div {
		display: flex;
		flex-direction: column;
	}

	.top-pawn-div {
		width: 100%;
		height: 30%;
		display: flex;
		flex-direction: column;
	}

	.name {
		color: hsla(0, 0%, 100%, .5);
	}
</style>
