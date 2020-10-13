<template>
	<view class="tabs">
		<scroll-view id="tab-bar" scroll-with-animation class="scroll-h" :scroll-x="true" :show-scrollbar="false"
		 :scroll-into-view="scrollInto">
			<view v-for="(tab, index) in tabBars" :key="tab.id" class="tab-item" :id="tab.id" :data-current="index" @click="tabClick">
				<!-- #ifdef APP-PLUS -->
				<text class="tab-item-title" :class="{ 'tab-item-title-active': tabIndex == index }">{{ tab.name }}</text>
				<!-- #endif -->

				<!-- #ifndef APP-PLUS -->
				<view class="tab-item-title" :class="{ 'tab-item-title-active': tabIndex == index }">{{ tab.name }}</view>
				<!-- #endif -->
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabIndex: 0,
				tabBars: [{
						name: '推荐',
						id: 'tab1'
					},
					{
						name: '种植',
						id: 'tab2'
					},
					{
						name: '养殖',
						id: 'tab3'
					},
					{
						name: '特色',
						id: 'tab4'
					},
					{
						name: '游玩',
						id: 'tab5'
					},
				],
				scrollInto: '',
			};
		},
		onLoad() {},
		methods: {
			tabClick(e) {
				let index = e.target.dataset.current || e.currentTarget.dataset.current;
				this.switchTab(index);
			},
			switchTab(index) {
				if (this.tabIndex === index) return;
				if (this.newsList[index].data.length === 0) {
					this.getList(index);
				}
				// 缓存 tabId
				if (this.newsList[this.tabIndex].pageIndex > MAX_CACHE_PAGEINDEX) {
					let isExist = this.cacheTab.indexOf(this.tabIndex);
					if (isExist < 0) {
						this.cacheTab.push(this.tabIndex);
						//console.log("cache index:: " + this.tabIndex);
					}
				}
			
				this.tabIndex = index;
				let scrollIndex = index - 1 < 0 ? 0 : index - 1;
				this.scrollInto = this.tabBars[scrollIndex].id;
			
				// 释放 tabId
				if (this.cacheTab.length > MAX_CACHE_PAGE) {
					let cacheIndex = this.cacheTab[0];
					this.clearTabData(cacheIndex);
					this.cacheTab.splice(0, 1);
					//console.log("remove cache index:: " + cacheIndex);
				}
			},
			
		}
	};
</script>

<style>
	/* #ifndef APP-PLUS */
	page {
		width: 100%;
		min-height: 100%;
		display: flex;
	}

	/* #endif */

	.tabs {
		flex: 1;
		flex-direction: column;
		overflow: hidden;
		background-color: #fafafa;
		/* #ifdef MP-ALIPAY || MP-BAIDU */
		height: 100vh;
		/* #endif */
	}

	.scroll-h {
		width: 750rpx;
		height: 80rpx;
		background-color: #ffffff;
		flex-direction: row;
		/* #ifndef APP-PLUS */
		white-space: nowrap;
		/* #endif */
		/* #ifdef H5 */
		position: fixed;
		top: 44px;
		left: 0;
		z-index: 999;
		/* #endif */
	}

	.tab-item {
		/* #ifndef APP-PLUS */
		display: inline-block;
		/* #endif */
		flex-wrap: nowrap;
		padding-left: 34rpx;
		padding-right: 34rpx;
	}

	.tab-item-title {
		color: #555;
		font-size: 30rpx;
		height: 80rpx;
		line-height: 80rpx;
		flex-wrap: nowrap;
		/* #ifndef APP-PLUS */
		white-space: nowrap;
		/* #endif */
	}

	.tab-item-title-active {
		color: #65D728;
		font-size: 36rpx;
		font-weight: bold;
		/* border-bottom-width: 6rpx;
		border-style: solid;
		border-color: #65D728; */
		text-align: center;
	}
</style>
