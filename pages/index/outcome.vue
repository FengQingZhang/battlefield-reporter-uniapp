<template>
	<view class="content">
		<van-notify id="van-notify" />
		<aui-loading ref="aui-loading" :show="auiLoading.show" :type="auiLoading.type" :direction="auiLoading.direction"
			:msg="auiLoading.msg" :mask="auiLoading.mask">
		</aui-loading>
		<aui-loading ref="aui-loading2" :show="auiLoading2.show" :type="auiLoading2.type"
			:direction="auiLoading2.direction" :msg="auiLoading2.msg" :mask="auiLoading2.mask">
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
							<view style="margin-left: 20rpx;">
								<van-checkbox :value="checked" shape="square" @change="onChange" label-class="concernClass">
								</van-checkbox>
							</view>
							<view style="color:#FFFFFF;" @click="onChange">
								关注
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<van-tabs ref="vantabs" active="overview" nav-class="my_nav" title-inactive-color="#FFFFFF"
			title-active-color="#FFFFFF" tab-class="my_tab_active" @click="tabs_click">
			<!-- 总览 -->
			<van-tab title="总览" name="overview">
				<view class="timeplayed-div">
					<view style="width: 10%;">
						<icon class="iconfont icon-youxi tab-icon" style="font-size: 10vw;"></icon>
					</view>
					<view class="timeplayed-text-div" style="width: 80%;margin-left:3%;">
						<view style="color: #FFFFFF;">游戏时长</view>
						<view style="color: #78b5e2;">{{timePlayed|timePlayed_filter}}</view>
					</view>
				</view>
				<van-grid square column-num="4" custom-class="grid-bg">
					<van-grid-item :key='index' use-slot v-for="(item,index) in overview">
						<ul>
							<li style="color: #78b5e2;">{{item.label}}</li>
							<li style="color: #efefef;font-size: 1.25rem;white-space: nowrap;">{{item.value}}</li>
						</ul>
					</van-grid-item>
				</van-grid>
				<view class="top-pawn-div"
					style="background-image:url(https://trackercdn.com/cdn/tracker.gg/bfv/top-class-bg.jpg);background-size: 100% 100%;">
					<view class="mask">
						<!-- 						<van-skeleton title row="3" :loading="loading">
 -->
						<view style="width: 100%;display: flex;flex-direction:row;">
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
												<li style="color: hsla(0,0%,100%,.5);font-size: large;">
													<span>兵种等级</span>
												</li>
												<li style="color: #efefef;font-size: x-large;white-space: nowrap;">
													{{topArm.stats.rank.displayValue}}
												</li>
											</ul>
										</van-col>
										<van-col span="10">
											<ul>
												<li style="color: hsla(0,0%,100%,.5);font-size: large;">SPM</li>
												<li style="color: #efefef;font-size: x-large;white-space: nowrap;">
													{{topArm.stats.scorePerMinute.displayValue}}
												</li>
											</ul>
										</van-col>
									</van-row>
									<van-row gutter="20">
										<van-col span="10">
											<ul>
												<li style="color: hsla(0,0%,100%,.5);font-size: large;">
													{{topArm.stats.kdRatio.displayName}}
												</li>
												<li style="color: #efefef;font-size:x-large;white-space: nowrap;">
													{{topArm.stats.kdRatio.displayValue}}
												</li>
											</ul>
										</van-col>
										<van-col span="10">
											<ul>
												<li style="color: hsla(0,0%,100%,.5);font-size: large;">KPM</li>
												<li style="color: #efefef;font-size:x-large;white-space: nowrap;">
													{{topArm.stats.killsPerMinute.displayValue}}
												</li>
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
						<view class="content"
							style="background-color: #1B2640;color: #FFFFFF;display: flex;flex-direction: column;">
							<view class="arm_div">
								<view> 时长: {{item.stats.timePlayed.displayValue}} </view>
								<view style="margin-left: 5%;"> 击杀数: {{item.stats.kills.displayValue}}</view>
								<view style="margin-left: 5%;"> K/D: {{item.stats.kdRatio.displayValue}} </view>
							</view>
							<view class="arm_div">
								<view> KPM: {{item.stats.killsPerMinute.displayValue}}</view>
								<view style="margin-left: 5%;"> 得分: {{item.stats.score.displayValue}} </view>
								<view style="margin-left: 5%;"> SPM: {{item.stats.scorePerMinute.displayValue}} </view>
							</view>
						</view>
					</uni-collapse-item>
				</uni-collapse>
				<view
					style="background-color: #1b2640;color:hsla(0,0%,100%,.5);width: 100%;height:5%;text-align: center;">
					这就是底线
				</view>
			</van-tab>
			<!-- 枪械 -->
			<van-tab title="枪械" name="weapons">
				<view v-show="weapons_show">
					<view class="best-div" style="background: url(https://trackercdn.com/cdn/tracker.gg/bfv/weapon-bg.jpg);background-size:cover;background-position: 50%;
						border-bottom: 2rpx solid #F0E68C;border-top: 2rpx solid #F0E68C;">
						<view class="mask">
							<view style="width: 99%;display: flex;flex-direction:row;">
								<view style="width: 10%;">
									<icon class="iconfont icon-jinpai" style="font-size: 10vw;color: #F0E68C;"></icon>
								</view>
								<view style="width: 85%;color: #efefef;font-size: large;padding-top: 2%;">
									使用最多
								</view>
							</view>
							<view style="width: 99%;display: flex;flex-direction: row;">
								<view style="width: 21%;text-align: center;">
									<van-image width="4rem" height="5rem" fit="contain" :src="topWeapon.imageUrl" />
								</view>
								<view style="width: 75%;">
									<van-row style="color:#efefef;">
										<span v-text="topWeapon.name"></span>
									</van-row>
									<van-row gutter="5" style="color:#78b5e2;font-size: x-small;">
										<van-col span="6">
											击杀数
										</van-col>
										<van-col span="6">
											KPM
										</van-col>
										<van-col span="6">
											时间
										</van-col>
										<van-col span="6">
											射击
										</van-col>
									</van-row>
									<van-row gutter="5" style="color: #FFFFFF;font-size: small;">
										<van-col span="6">
											<span v-text="topWeapon_info_arr[0].displayValue"></span>
										</van-col>
										<van-col span="6">
											<span v-text="topWeapon_info_arr[1].displayValue"></span>
										</van-col>
										<van-col span="6">
											<span v-text="topWeapon_info_arr[2].displayValue"></span>
										</van-col>
										<van-col span="6">
											<span v-text="topWeapon_info_arr[3].displayValue"></span>
										</van-col>
									</van-row>
									<van-row gutter="5" style="color:#78b5e2;font-size: x-small;">
										<van-col span="6">
											命中
										</van-col>
										<van-col span="6">
											命中率
										</van-col>
										<van-col span="6">
											爆头
										</van-col>
										<van-col span="6">
											爆头率
										</van-col>
									</van-row>
									<van-row gutter="5" style="color: #FFFFFF;font-size: small;">
										<van-col span="6">
											<span v-text="topWeapon_info_arr[4].displayValue"></span>
										</van-col>
										<van-col span="6">
											<span v-text="topWeapon_info_arr[5].displayValue"></span>%
										</van-col>
										<van-col span="6">
											<span v-text="topWeapon_info_arr[6].displayValue"></span>
										</van-col>
										<van-col span="6">
											<span v-text="topWeapon_info_arr[7].Value"></span>
										</van-col>
									</van-row>
								</view>
							</view>
						</view>
					</view>
				</view>
				<view style="background-color: #1b2640;color: #FFFFFF;" v-show="weapons_show" :style="{height:screen_height*0.65+'px'}">
					<z-paging ref="paging" v-model="weapons_list" loading-more-no-more-text="我也是有底线的！" @query="queryList" :fixed="false">
						<view v-for="(item,index) in weapons_list" :key='index' style=" display:flex; flex-direction: row;">
							<view style="width: 20%;text-align: center;border-bottom: 1px solid #78b5e2;">
								<van-image width="4rem" height="5rem" fit="contain"  lazy-load :src="item.metadata.imageUrl" />
							</view>
							<view style="display: flex;flex-direction:column;width: 80%;border-bottom: 1px solid #78b5e2 ;">
								<view>
									<span v-text="item.metadata.name"></span>
								</view>
								<view>
									<van-row style="color:#78b5e2;font-size:small;">
										<van-col span="6">
											击杀数
										</van-col>
										<van-col span="6">
											KPM
										</van-col>
										<van-col span="6">
											时间
										</van-col>
										<van-col span="6">
											射击
										</van-col>
									</van-row>
									<van-row style="color:#FFFFFF;font-size:x-small;">
										<van-col span="6">
											<span v-text="item.stats[0].displayValue"></span>
										</van-col>
										<van-col span="6">
											<span v-text="item.stats[1].displayValue"></span>
										</van-col>
										<van-col span="6">
											<span v-text="item.stats[2].displayValue"></span>
										</van-col>
										<van-col span="6">
											<span v-text="item.stats[3].displayValue"></span>
										</van-col>
									</van-row>
									<van-row style="color:#78b5e2;font-size:small;">
										<van-col span="6">
											命中
										</van-col>
										<van-col span="6">
											命中率
										</van-col>
										<van-col span="6">
											爆头
										</van-col>
										<val-col span="6">
											爆头率
										</val-col>
									</van-row>
									<van-row style="color:#FFFFFF;font-size: x-small;">
										<van-col span="6">
											<span v-text="item.stats[4].displayValue"></span>
										</van-col>
										<van-col span="6">
											<span v-text="item.stats[5].displayValue"></span>%
										</van-col>
										<van-col span="6">
											<span v-text="item.stats[6].displayValue"></span>
										</van-col>
										<van-col span="6">
											{{item.stats[6].value/item.stats[0].value|headshort_per_filter}}
										</van-col>
									</van-row>
								</view>
							</view>
						</view>
					</z-paging>
				
					<!-- <view
						style="background-color: #1b2640;color:hsla(0,0%,100%,.5);width: 100%;height:5%;text-align: center;">
						没有更多了
					</view> -->
				</view>
			</van-tab>
			<van-tab title="载具" name="vehicles">
				<view v-show="vehicles_show">
					<view class="best-div" style="background: url(https://trackercdn.com/cdn/tracker.gg/bfv/vehicle-bg.jpg);background-size:cover;background-position: 50%;
						border-bottom: 2rpx solid #F0E68C;border-top: 2rpx solid #F0E68C;">
						<view class="mask">
							<view style="width: 99%;display: flex;flex-direction:row;">
								<view style="width: 10%;">
									<icon class="iconfont icon-jinpai" style="font-size: 10vw;color: #F0E68C;"></icon>
								</view>
								<view style="width: 85%;color: #efefef;font-size: large;padding-top: 2%;">
									使用最多
								</view>
							</view>
							<view style="width: 99%;display: flex;flex-direction: row;">
								<view style="width: 21%;text-align: center;">
									<van-image width="4rem" height="5rem" fit="contain"
										:src="topVehicle.metadata.imageUrl" />
								</view>
								<view style="width: 75%;">
									<van-row>
										<view style="color:#efefef;margin-bottom:1rpx">
											<span v-text="topVehicle.metadata.name"></span>
										</view>
									</van-row>
									<van-row gutter="5" style="color:#78b5e2;font-size:large;">
										<van-col span="8">
											击杀数
										</van-col>
										<van-col span="8">
											KPM
										</van-col>
										<van-col span="8">
											时间
										</van-col>
									</van-row>
									<van-row gutter="5" style="color: #FFFFFF;font-size:medium;">
										<van-col span="8">
											<span v-text="topVehicle.stats[0].displayValue"></span>
										</van-col>
										<van-col span="8">
											<span v-text="topVehicle.stats[1].displayValue"></span>
										</van-col>
										<van-col span="8">
											<span v-text="topVehicle.stats[2].displayValue"></span>
										</van-col>
									</van-row>
								</view>
							</view>
						</view>
					</view>
				</view>
				<view style="background-color: #1b2640;color: #FFFFFF;padding: 10rpx;" v-show="vehicles_show">
					<view v-for="(item,index) in vehicles_arr" :key='index' style=" display:flex; flex-direction: row;">
						<view style="width: 20%;text-align: center;border-bottom: 1px solid #78b5e2;">
							<van-image width="4rem" height="5rem" lazy-load fit="contain" :src="item.metadata.imageUrl" />
						</view>
						<view style="display: flex;flex-direction:column;width: 80%;border-bottom: 1px solid #78b5e2 ;">
							<view>
								<span v-text="item.metadata.name"></span>
							</view>
							<view>
								<van-row style="color:#78b5e2;font-size:large;">
									<van-col span="6">
										击杀数
									</van-col>
									<van-col span="6">
										KPM
									</van-col>
									<van-col span="6">
										时间
									</van-col>
								</van-row>
								<van-row style="color:#FFFFFF;font-size:medium;">
									<van-col span="6">
										<span v-text="item.stats[0].displayValue"></span>
									</van-col>
									<van-col span="6">
										<span v-text="item.stats[1].displayValue"></span>
									</van-col>
									<van-col span="8">
										<span v-text="item.stats[2].displayValue"></span>
									</van-col>
								</van-row>
							</view>
						</view>
					</view>
					<view
						style="background-color: #1b2640;color:hsla(0,0%,100%,.5);width: 100%;height:5%;text-align: center;">
						没有更多了
					</view>
				</view>
	</van-tab>
	<van-tab title="对局" name="rank">
		<view style="background-color: #1b2640;color: #FFFFFF;" v-show="rank_show">
			<view v-for="(item,index) in rank_arr" :key="index" class="rank-div" :style="{backgroundImage:'url('+item.map.imageUrl+')'}">
				<view style="width: 100%;display: flex;flex-direction: column;">
					<view class="mask">
						<view style="margin: 10rpx;font-size: medium;">
							地图：{{item.mapKey|map_usTocn}}
						</view>
						<view style="margin: 10rpx;font-size: medium;">
							时间：{{item.timestamp|timestamp_filter}}
						</view>
						<view style="margin: 10rpx;font-size: medium;">
							服务器：{{item.serverName}}
						</view>
					</view>
				</view>
			</view>
			<view v-show="!error_show"
				style="background-color: #1b2640;color:hsla(0,0%,100%,.5);width: 100%;height:5%;text-align: center;">
				没有更多了
			</view>
		</view>
		<view v-if="error_show"
			style="color: #1b2640;width: 100%;height:100%;display: flex;justify-content: center;align-items: center;text-align: center;flex-direction: column;">
			<view style="margin-top: 10rem;">
				<van-image width="6rem" height="6rem" fit="fill" :src="error_cry"/>
			</view>
			<view style="text-align: center;">
				服务器出现错误，请稍后再试
			</view>
		</view>
	</van-tab>
	</van-tabs>
	</view>
</template>

<script>
	import auiLoading from '@/components/aui-loading/aui-loading.vue';
	import Notify from '@/wxcomponents/vant/dist/notify/notify';
	import url from '../../common/config.js';
	import map from '../../common/map_cn.js';
	var overall;
	export default {
		components: {
			auiLoading
		},
		data() {
			return {
				checked:false,
				head_src: '',
				username: '',
				level: '',
				timePlayed: '',
				platformSlug: '',
				overview: [],
				armScore: [],
				topArm: '',
				//全屏的载入框
				auiLoading: {
					show: false,
					type: 4,
					direction: 'col',
					msg: '加载中',
					mask: false,
				},
				auiLoading2: {
					show: false,
					type: 3,
					direction: 'col',
					msg: '加载中',
					mask: true,
				},
				weapon_flag: false, //weapon页面是否查询标识，
				vehicle_flag: false, //载具是否已经查询标识
				rank_flag: false, //
				topWeapon: '', //使用最多的武器
				topWeapon_info_arr: [],//最多武器的信息
				weapons_arr: [],
				weapons_list:[],
				weapons_total_page:'',//武器总页数
				topVehicle: '',
				vehicles_arr: [],
				weapons_show: false,
				vehicles_show: false,
				rank_arr:[],
				rank_show: false,
				error_show:false,//错误图标是否显示
				error_cry:url.errer_image.cry,//错误图标url
				screen_height:'',//屏幕高度
				favorite:[],
				dist:'',
			}
		},
		methods: {
			//获取总览信息
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
								if (segments[i].metadata.name == 'Assault') {
									segments[i].metadata.name = '偷鸡兵'
								} else if (segments[i].metadata.name == "Medic") {
									segments[i].metadata.name = '兽医'
								} else if (segments[i].metadata.name == "Support") {
									segments[i].metadata.name = '支援兵'
								} else if (segments[i].metadata.name == 'Recon') {
									segments[i].metadata.name = '斟茶兵'
								} else if (segments[i].metadata.name == 'Pilot') {
									segments[i].metadata.name = '沙扣分行员'
								} else if (segments[i].metadata.name == 'Tanker') {
									segments[i].metadata.name = '沙扣坦克驾驶员'
								}
								this.armScore.push(segments[i]);
							}
						}
						this.getTopArm();
						this.level = data.rank.value;
						this.timePlayed = data.timePlayed.displayValue;
						let longes = parseInt(data.longestHeadshot.value);
						this.overview.push({
							'label': 'SPM',
							'value': data.scorePerMinute.displayValue
						}, {
							'label': 'KPM',
							'value': data.killsPerMinute.displayValue
						}, {
							'label': 'K/D',
							'value': data.kdRatio.displayValue
						}, {
							'label': '命中率',
							'value': data.shotsAccuracy.displayValue
						}, {
							'label': '胜率',
							'value': data.wlPercentage.displayValue
						}, {
							'label': '击杀数',
							'value': data.kills.displayValue
						}, {
							'label': '死亡数',
							'value': data.deaths.displayValue
						}, {
							'label': '爆头数',
							'value': data.headshots.displayValue
						}, {
							'label': '爆头率',
							'value': headshots_rate
						}, {
							'label': '胜利场次',
							'value': data.wins.displayValue
						}, {
							'label': '场均击杀',
							'value': kill_rate
						}, {
							'label': '场均死亡',
							'value': death_rate
						}, {
							'label': '最远爆头',
							'value': longes
						}, {
							'label': '最高连杀',
							'value': data.killStreak.displayValue
						}, {
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
					if (score < this.armScore[i].stats.score.value) {
						score = this.armScore[i].stats.score.value;
						temp = this.armScore[i];
					}
				}
				this.topArm = temp;
				this.$refs['aui-loading'].hide();
			},
			//tab的点击事件
			tabs_click(even) {
				let name = even.target.name;
				if (name == 'weapons' && !this.weapon_flag) {
					if (!this.weapons_show) {
						this.$refs['aui-loading2'].show();
					}
					uni.request({
						url: url.EA.weapons_vehicles_url + this.platformSlug + "/" + this.username + "/weapons",
						success: (res) => {
							this.weapons_data_parse(res.data);
							this.weapon_flag=true;
						},
						fail: () => {
							console.log("失败");
						}
					});
				} else if (name == 'vehicles' && !this.vehicle_flag) {
					if (!this.vehicles_show) {
						this.$refs['aui-loading2'].show();
					}
					uni.request({
						url: url.EA.weapons_vehicles_url + this.platformSlug + "/" + this.username + "/vehicles",
						success: (res) => {
							this.vehicles_data_parse(res.data);
							this.vehicle_flag=true;
						},
						fail: () => {
							console.log("失败");
						}
					});
				} else if (name == 'rank' && !this.rank_flag) {
					if (!this.rank_show) {
						this.$refs['aui-loading2'].show();
					}
					uni.request({
						url: url.EA.report_url + this.platformSlug + "/latest/" + this.username,
						success: (res) => {
							this.report_data_parse(res);
							this.rank_flag=true;
						},
						fail: () => {
							console.log("失败");
						}
					});
				}
			},
			//武器数据解析
			weapons_data_parse(res) {
				this.weapons_arr = res.data.children.sort(function(a, b) {
					return b.stats[2].value - a.stats[2].value;
				});
				this.topWeapon = this.weapons_arr[0].metadata;
				this.topWeapon_info_arr = this.weapons_arr[0].stats;
				let headshort_pre = (this.topWeapon_info_arr[6].value / this.topWeapon_info_arr[0].value) * 100;
				headshort_pre = headshort_pre.toPrecision(3) + "%";
				this.topWeapon_info_arr.push({
					Value: headshort_pre
				});
				this.weapons_arr.splice(0,1);
				let length = this.weapons_arr.length;
				//计算总页数
				this.weapons_total_page= Math.ceil(length/10);
				this.queryList(1,10);
				let that = this;
				setTimeout(function() {
					that.$refs['aui-loading2'].hide();
					that.weapons_show = true;
				},800)
			},
			//载具数据解析
			vehicles_data_parse(res) {
				this.vehicles_arr = res.data.children.sort(function(a, b) {
					return b.stats[2].value - a.stats[2].value;
				});
				this.topVehicle = this.vehicles_arr[0];
				this.vehicles_arr.splice(0,1);
				let that = this;
				setTimeout(function() {
					that.$refs['aui-loading2'].hide();
					that.vehicles_show = true;
				}, 500)
			},
			//战绩数据解析
			report_data_parse(res){
				let data =res.data.data; 
				let that = this;
				if(typeof(data) == "undefined"){
					Notify({type:'danger',message:'EA服务器开小差了，请稍后再试'});
					that.$refs['aui-loading2'].hide();
					that.rank_show = true;
					that.error_show=true;
					return false;
				}else{
					this.error_show=false;
					this.rank_arr=data.reports;
					console.log(this.rank_arr);
					setTimeout(function(){
						that.$refs['aui-loading2'].hide();
						that.rank_show = true;
					},600)
				}
				
			},
			queryList(pageNo,pageSize){
				//console.log("当前页数="+pageNo);
				//console.log("总页数="+this.weapons_total_page);
				//判断当前页码是否大于总页码
				if(pageNo<=this.weapons_total_page){
					if(this.weapons_arr.length>0){
						let temp=[];
						for(let i=0;i<pageSize;i++){
							if(this.weapons_arr[i]){
								temp.push(this.weapons_arr[i]);
							}else{
								break;
							}
						}
						this.weapons_arr.splice(0,10);
						this.$refs['paging'].complete(temp);
					}
					// this.$refs['paging'].complete([1,2,3,4,5,6,7,8,9,20]);
				}
				
			},
			//点击关注时的操作
			onChange(event) {
			   if(this.checked){
				   this.checked=false;
				   let data = this.favorite;
				   let index = data.indexOf(this.username);
				   data.splice(index,1);
				   uni.setStorage({
				   	key: "favorite",
				   	data: data
				   })
			   }else{
				   this.checked=true;
				   this.favorite.push(this.username);
				   let data = this.favorite;
				   uni.setStorage({
				   	key: "favorite",
				   	data: data
				   })
			   }
			},
			//异步更新数据
			// onLoad(){
			// 	if(this.weapons_arr.length==0){
			// 		this.loading=false;
			// 		this.finished=true;
			// 	}else{
			// 		for(let i=0;i<this.weapons_arr.length;i++){
			// 			this.weapons_list.push(this.weapons_arr[i]);
			// 			this.weapons_arr[i].delete();
			// 		}
			// 	}
			// }
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
			},
			headshort_per_filter(val) {
				val = val * 100;
				return val.toPrecision(3) + '%';
			},
			timestamp_filter(val){
				let date = new Date(val*1000);
				let year = date.getFullYear(),
				                    month = date.getMonth() + 1,
				                    sdate = date.getDate(),
				                    hour = date.getHours(),
				                    minute = date.getMinutes(),
				                    second = date.getSeconds();
				let result = year + "-"+ month +"-"+ sdate +" "+ hour +":"+ minute +":" + second;
				return result;
			},
			map_usTocn(val){
				if(overall.dist.has(val)){
					return overall.dist.get(val);
				}else{
					return val;
				}
			}
		},
		onLoad(option) {
			this.$refs['aui-loading'].show();
			this.head_src = option.avatarUrl;
			this.username = option.username;
			this.platformSlug = option.platformSlug;
			this.getInfoByName();
			this.$refs['vantabs'].resize();
		},
		onUnload() {
			uni.setStorageSync("goBackIndex",1);
		},
		mounted() {
			let that = this;
			uni.getSystemInfo({
				success(res) {
					that.screen_height = res.windowHeight;
				}
			})
		},
		created() {
			overall =this;
			this.dist = new Map();
			let mapData = map.dist;
			for(let i=0;i<mapData.length;i++){
				this.dist.set(mapData[i].label,mapData[i].value);
			}
			uni.getStorage({
				key:'favorite',
				success:(res)=>{
					this.favorite = res.data;
					if(this.favorite.indexOf(this.username)>-1){
						this.checked=true;
					}else{
						this.checked=false;
					}
				},
				fail: () => {
					/* 没取到的操作 */
				}
			})
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
	.my_tab_active{
		height: 86%;
		width: 100%;
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

	.arm_div {
		display: flex;
		flex-direction: row;
		padding-left: 5%;
	}

	.best-div {
		width: 100%;
		height: 10%;
		display: flex;
		flex-direction: column;
	}
	.rank-div{
		width: 100%;
		height: auto;
		display: flex;
		flex-direction: row;
		background-size:cover;
		background-position: 50%;
		border-bottom: 2rpx solid #F0E68C;border-top: 2rpx solid #F0E68C;
	}
	.grid-bg{
		background-color: #1b2640;
	}
	.concernClass{
		color: #FFFFFF;
	}
</style>
