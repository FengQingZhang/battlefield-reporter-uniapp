<template style="height:100%;">
	<view class="content">
		<uni-popup ref="popup" type="message">
			<uni-popup-message type="warn" :message="message" :duration="2000"></uni-popup-message>
		</uni-popup>
		<van-dialog id="van-dialog"></van-dialog>
		<view style="height: 50%;">
			<view class="text-area">
				<view class="background-mask">
					<view class="uni-flex uni-row top-div">
						<view class="flex-item mark">V</view>
						<view class="flex-item top-title-b typeface">Battlefield 5 Stats</view>
					</view>
					<view style="height: 20%;width:100%;margin-top: 7%;">
						<view id="tab">
							<ul>
								<li :class="platform=='origin'?'active':''" @click='changeTab(0)'>
									<icon class="iconfont icon-origin tab-icon"></icon>
								</li>
								<li :class="platform=='xbl'?'active':''" @click="changeTab(1)">
									<icon class="iconfont icon-xbox tab-icon"></icon>
								</li>
								<li :class="platform=='psn'?'active':''" @click="changeTab(2)">
									<icon class="iconfont icon-PS tab-icon"></icon>
								</li>
							</ul>
						</view>
					</view>
					<view ref="searchdiv" id='searchdiv' style="width: 85%;margin: 0 auto; margin-top: 5%;height: 50%;">
						<uni-search-bar :placeholder="placheholder" @focus="showHistory" :radius="5" @confirm="search"
							:cancelButton="none" class="square_radius" v-model="search_content">
						</uni-search-bar>
						<view v-show="show" class="search-history">
							<!-- <uni-icons type="close" size="25" style="float: right;" @click="closeHistory"> -->
							<van-tabs ref="vantabs" active="a" swipeable color='#f0674e'>
								<van-tab title="我的关注" name="a">内容 1</van-tab>
								<van-tab title="搜索历史" name="b">
									<ul style="padding: 10rpx;">
										<li v-for="(item,index) in search_history" :key='index'
											style="display: inline; margin-left: 10rpx;">
											<van-tag closeable size="large" type='primary' @close.stop='del(item)'>
												<a @click.stop='chooseHistory(item)'> {{item}}</a>
											</van-tag>
										</li>
									</ul>
								</van-tab>
							</van-tabs>
						</view>
					</view>
				</view>
				<view class="triangle">
					<view class="triangle-mask"></view>
				</view>
			</view>
		</view>
		<view class="strategy-div">
			<view class="strategy-center-div">
				<view class="strategy-row">
					<view class="strategy-row-picture-div">
						<van-image
						  width="6rem"
						  height="5rem"
						  fit="contain"
						  src="http://qxcjfm2yn.hb-bkt.clouddn.com/STG44.png"
						/>
					</view>
					<view class="strategy-row-text-div"
						style="background:url(/static/index/m1.jpg);background-size: 100% 100%;border-radius: 0 0 5px 5px;">
						<view class="strategy-row-text-mask">
							<view class="typeface strategy-row-text">枪械加点</view>
						</view>
					</view>
				</view>
				<view class="strategy-row">
					<view class="strategy-row-picture-div">
						<van-image
						  width="5rem"
						  height="5rem"
						  fit="contain"
						  src="http://qxcjfm2yn.hb-bkt.clouddn.com/LVT.png"
						/>
					</view>
					<view class="strategy-row-text-div"
						style="background:url(/static/index/dogtank.jpeg);background-size: 100% 100%;border-radius: 0 0 5px 5px;">
						<view class="strategy-row-text-mask">
							<view class="typeface strategy-row-text">坦克加点</view>
						</view>
					</view>
				</view>
				<view class="strategy-row">
					<view class="strategy-row-picture-div">
						<van-image
						  width="4rem"
						  height="5rem"
						  fit="contain"
						  src="http://qxcjfm2yn.hb-bkt.clouddn.com/JU88A.png"
						/>
					</view>
					<view class="strategy-row-text-div"
						style="background: url(/static/index/f4u.jpg);background-size: 100% 100%;border-radius: 0 0 5px 5px;">
						<view class="strategy-row-text-mask">
							<view class="typeface strategy-row-text">飞机加点</view>
						</view>
					</view>
				</view>
			</view>
			<view style="position: absolute; bottom: 0; width:80%;height: 10%; text-align: center; color: #f0674e;">
				Powered By DubboCat
			</view>
		</view>
	</view>
</template>

<script>
	import Dialog from '../../wxcomponents/vant/dist/dialog/dialog';
	import url from '../../common/config.js'
	export default {
		data() {
			return {
				title: 'Hello',
				search_content: '',
				platform: "origin",
				placheholder: "请输入origin用户名",
				hint: [
					"请输入origin用户名",
					"请输入xbox用户名",
					"请输入PS用户名"
				],
				platforms: [
					"origin",
					"xbl",
					"psn",
				],
				message: '',
				show: false,
				tab_show:false,
				search_history: []
			}
		},
		onLoad() {

		},
		methods: {
			search() {
				if (this.search_content == '') {
					this.message = '用户名不能为空';
					this.$refs['popup'].open();
					return false;
				}
				let that = this;
				uni.request({
					url: url.EA.find_user_url + "platform=" + this.platform +
						"&query=" + this.search_content,
					success: (res) => {
						//console.log(res.data);
						let temp = res.data;
						if (temp.data.length == 0) {
							Dialog.alert({
								context: this,
								title: '查询失败',
								message: '您查询的用户名不存在',
							}).then(() => {
								// close
							})
							return false;
						}
						let name = temp.data[0].platformUserHandle;
						let platformSlug = temp.data[0].platformSlug;
						let avatarUrl = temp.data[0].avatarUrl;
						if (this.search_history.indexOf(name) == -1) {
							this.search_history.push(name);
							let data = this.search_history;
							uni.setStorage({
								key: "search_history",
								data: data
							})
						}
						uni.navigateTo({
							url: 'outcome?username=' + name + '&avatarUrl=' + avatarUrl+'&platformSlug='+platformSlug,
						})
					},
					fail: () => {
						console.log("请求失败");
					}
				})
			},
			changeTab(index) {
				this.platform = this.platforms[index];
				this.placheholder = this.hint[index];
				this.closeHistory();
			},
			showHistory() {
				if (this.search_history.length > 0) {
					 this.show = true;
					 this.$nextTick(() => {
					    this.$refs['vantabs'].resize();
					});
				}
			},
			closeHistory() {
				//console.log(currentCli)
				/* if(currentCli){
					if(!currentCli.contains(event.target)){
						this.show = false;
					}
				} */
				this.show = false;
			},
			del(name) {
				let index = this.search_history.indexOf(name);
				this.search_history.splice(index, 1);
			},
			chooseHistory(name) {
				this.search_content = name;
				this.closeHistory();
			}
		},
		created() {
			uni.getStorage({
				key: 'search_history',
				success: (res) => {
					this.search_history = res.data;
				},
				fail: () => {
					/* 没取到的操作 */
				}
			})
		}
	}
</script>

<style>
	/*标志css*/
	.mark {
		background-color: #f0674e;
		width: 11%;
		height: 60%;
		margin-top: 2%;
		margin-left: 1%;
		text-align: center;
		color: white;
		font-size: 7vw;
		border-radius: 25%;
	}

	.content {
		width: 100%;
		height: 100%;
	}

	/*字体*/
	.typeface {
		font-family: Rajdhani,
			Roboto, -apple-system,
			BlinkMacSystemFont, Segoe UI,
			Oxygen-Sans, Ubuntu, Cantarell,
			Helvetica, Arial, sans-serif, Apple Color Emoji,
			Segoe UI Emoji, Segoe UI Symbol;
	}

	/*背景*/
	.text-area {
		width: 100%;
		height: 94%;
		/*  background: url(https://trackercdn.com/cdn/tracker.gg/bfv/bfv-profile-hero1.jpg) no-repeat;*/
		background: url(../../static/index/image2.jpg);
		background-size: 100% 100%;
		/* border-radius: 0 57px; */
		position: relative
	}

	.top-div {
		width: 100%;
		height: 20%;
		display: flex;
		flex-direction: row;
		margin-top: 3%;
	}

	/*图片遮罩*/
	.background-mask {
		display: flex;
		flex-direction: column;
		background: rgba(0, 0, 0, .4);
		width: 100%;
		height: 100%;
	}

	/*头部文字*/
	.top-title-b {
		margin-top: 2%;
		margin-left: 2%;
		color: white;
		font-size: 8vw;
	}

	/*圆角属性*/
	.square_radius {
		border-radius: 25px;
	}

	/*选择条件区域 */
	#tab {
		border-radius: 10px;
		width: 80%;
		height: 100%;
		margin: 0 auto;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	#tab ul {
		width: 100%;
		height: 100%;
		border-radius: 5px;
		background: rgb(27, 38, 64, 0.5);
		display: flex;
		justify-content: center;
		align-content: center;
	}

	#tab ul li {
		width: 100%;
		height: 100%;
		list-style: none;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	/* li 选中效果时背景颜色 */
	#tab ul li.active {
		border-radius: 5px;
		background-color: #253258;

	}

	/*选中时增加底边框*/
	#tab ul li.active icon {
		border-bottom-color: #f0674e;
	}

	/*xbox ps origin 图标样式*/
	.tab-icon {
		color: #FFFFFF;
		height: 60%;
		width: 1.8rem;
		border-top: 2px solid transparent;
		border-bottom: 2px solid transparent;
		fill: var(--text-color);
		padding: .5rem 0;
	}

	/*搜索历史背景渐变色*/
	.search-history-div {
		width: 100%;
		height: 40%;
		background-image: linear-gradient(90deg, #253258, #c4486f);
		z-index: -1;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.triangle {
		width: 100%;
		height: 7%;
		z-index: -1;
		background-image: linear-gradient(90deg, #253258, #c4486f);
	}

	.triangle-mask {
		width: 100%;
		height: 100%;
		background: rgba(0, 0, 0, .4);
	}

	.strategy-div {
		display: flex;
		justify-content: center;
		width: 100%;
		height: 50%;
		/* background-image: linear-gradient(90deg, #253258, #c4486f); */
		background-color: #0b121d;
		position: relative;
	}

	.strategy-center-div {
		width: 90%;
		height: 100%;
		display: flex;
		flex-direction: column;
	}

	.strategy-row {
		background-image: linear-gradient(-40deg, #cbb765, #665c33);
		height: 20%;
		width: 100%;
		border-radius: 5px;
		margin-top: 5%;
		display: flex;
		flex-direction: row;
	}

	.strategy-row-picture-div {
		height: 100%;
		width: 20%;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.strategy-row-picture {
		height: 100%;
		width: 20%;
	}

	.strategy-icon {
		height: 30%;
		color: #FFFFFF;
	}

	.strategy-row-text-div {
		width: 80%;
		height: 100%;

	}

	.strategy-row-text-mask {
		width: 100%;
		height: 100%;
		background: rgba(0, 0, 0, .4);
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.strategy-row-text {
		color: white;
		font-size: 8vw;
		font-family: fantasy;
	}

	.search-history {
		height: 30vw;
		width: 100%;
		background-color: #FFFFFF;
		float: inline-start;
		border-radius: 5px;
		overflow-y: scroll;
	}

	/*历史标签的div*/
	.history-tab-div {
		width: 100%;
		height: 100%;
		margin: 0 auto;
	}
</style>
