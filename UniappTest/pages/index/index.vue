<template>
	<view>
		<!-- 搜索框开始 -->
		<view class="cu-bar search bg-gradual-orange">
			<view class="search-form round">
				<text class="cuIcon-search"></text>
				<input v-model="search" placeholder="请输入您想要的商品" />
			</view>
			<view class="action">
				<text class="cuIcon-scan" style="font-size: 150%;"></text>
			</view>
		</view>
		<!-- 搜索框结束 -->
		<!-- 测试公司电脑 -->
		<!-- 导航栏Tab开始 -->
		<view>
			<scroll-view class="nav bg-white" scroll-x>
				<view class="flex text-center">
					<view :class="thisTab==index?'text-orange cur':''" @click="changeTab(index)" v-for="(item,index) in TabLists" :key="index"
					 class="cu-item flex-sub">
						{{item}}
					</view>
				</view>
			</scroll-view>
		</view>
		<!-- 导航栏Tab结束 -->
		<!-- 轮播图开始 -->
		<swiper style="margin-top: 5upx;" class="square-dot" :circular="true" :indicator-dots="true">
			<swiper-item v-for="(item,index) in bannerLists" :key="index">
				<image mode="scaleToFill" class="swiperimg" :src="item.picUrl"></image>
			</swiper-item>
		</swiper>
		<!-- 轮播图结束 -->
		<!-- 进入众筹开始 -->
		<view class="bg-white zhongchou">
			<view style="display: flex;align-items: center;">
				<image src="../../static/tab/car.png" style="width: 50upx;height: 50upx;"></image>
				<text style="margin-left: 20upx;">来众筹</text>
			</view>
			<view class="text-grey">查看全部<text class="cuIcon-right"></text></view>
		</view>
		<!-- 进入众筹结束 -->
		<!-- 导航图标开始 -->
		<view class="guide bg-white">
			<view v-for="(item,index) in guideLists" :key="index">
				<image class="guide-icon" :src="item.url"></image>
				<view class="guide-text">{{item.name}}</view>
			</view>
		</view>
		<!-- 导航图标结束 -->
		<!-- 百亿补贴开始 -->
		<view class="bg-white butie ">
			<view class="flex align-center justify-between">
				<view class="flex buleft align-center ">
					<image src="../../static/icon/money.png"></image>
					<text style="margin-left: 10upx;font-size: 120%;" class="text-bold">百亿补贴</text>
					<text style="margin-left: 10upx;font-size: 110;" class="cu-tag text-bold">大牌正品 官方补贴</text>
				</view>
				<view style="margin-right: 10upx;">
					<text>全部</text>
					<text class="cuIcon-right"></text>
				</view>
			</view>

			<view class="flex justify-around">
				<view v-for="i in 5" :key="i">
					<view class="flex justify-center" style="position: relative;">
						<image style="width: 150upx;height: 150upx;" src="../../static/oneplus8.jpg"></image>
						<view class="cu-tag bg-orange " style="position: absolute;bottom: 10upx;">补贴价</view>
					</view>
					<view style="text-align: center;">
						<text style="" class="text-orange text-bold">￥3999</text>
					</view>
				</view>
			</view>

		</view>
		<!-- 百亿补贴结束 -->
		<!-- 商品列表开始 -->
		<view>
			<view class="flex bg-white justify-around" style="margin-top: 5upx;padding: 8upx 8upx 8upx 8upx" v-for="(item,index) in goodLists.slice(0,5)"
			 :key="index">
				<view class="goods">
					<image :src="item.pic"></image>
				</view>
				<view class="flex justify-between" style="width: 400upx;flex-direction: column;">
					<view>
						<view class="bg-gradual-red text-center">
							{{item.name}}
						</view>
						<view class="text-black" style="margin-top: 30upx;">
							{{item.characteristic}}
						</view>
					</view>
					<view class="text-price text-red">
						{{item.minPrice}}
					</view>
				</view>
			</view>
		</view>
		<!-- 商品列表结束 -->
	</view>
</template>

<script>
	const WXAPI = require('apifm-wxapi')
	WXAPI.init('huamei')
	export default {
		data() {
			return {
				title: 'Hello',
				shu: null,
				search: "",
				TabLists: ["热门", "水果", "蔬菜", "肉类", "众筹"],
				thisTab: 0,
				guideLists: [{
						url: "../../static/icon/analyse.png",
						name: "财经分析"
					},
					{
						url: "../../static/icon/bank.png",
						name: "银行"
					},
					{
						url: "../../static/icon/rise.png",
						name: "上涨趋势"
					},
					{
						url: "../../static/icon/save.png",
						name: "存款"
					},
					{
						url: "../../static/icon/new.png",
						name: "新闻消息"
					}
				],
				bannerLists: [],
				goodLists: [],
			}
		},
		methods: {
			getBanners() {
				uni.request({
					url: 'https://api.it120.cc/huamei/banner/list',
					data: {
						type: 'index'
					},
					success: (res) => {
						this.bannerLists = res.data.data;
					}
				})
			},
			getGoods() {
				uni.request({
					url: 'https://api.it120.cc/huamei/shop/goods/list',
					data: {
						categoryId: '138915',
					},
					success: (res) => {
						this.goodLists = res.data.data;
					}
				}, )
			},
			change() {
				this.shu = "has clicked"
			},
			changeTab(index) {
				this.thisTab = index;
			},
		},
		onLoad() {
			this.getBanners()
			this.getGoods()
		},

	}
</script>

<style>
	.swiperimg {
		width: 750upx;
		height: 250upx;
	}

	.zhongchou {
		display: flex;
		justify-content: space-between;
		align-items: center;
		height: 90upx;
		padding-left: 20upx;
		padding-right: 20upx;
	}

	.guide-icon {
		width: 80upx;
		height: 80upx;
		margin-left: 20upx;
		margin-right: 20upx;
	}

	.guide {
		display: flex;
		align-items: center;
		justify-content: space-around;

	}

	.guide-text {
		font: bold;
		text-align: center;
	}

	.butie {
		margin-top: 10upx;
	}

	.buleft {

		margin-top: 20upx;
		margin-bottom: 20upx;

	}

	.buleft image {
		width: 60upx;
		height: 60upx;
		margin-left: 5upx;
	}

	.goods image {
		width: 250upx;
		height: 250upx;
	}
</style>
