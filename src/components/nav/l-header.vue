<template>
	<div class="header"
			 :style="{'background-color' : navStyle.backgroundColor}"
			 :class="[
			 {
			 	'header-fixed': fixedHeader,
			 	'header-fixed-min-width': pattern == 1 && fixedHeader,
			 	'header-width': pattern == 1 && !fixedHeader || pattern == 2
			 }
			 ]">

		<Header class="as-header"
						:style="{'color' : navStyle.textColor}"
						v-if="pattern == '1'">
			<div class="as-header-content">
				<LHeaderContent :is-show-search="isShowSearch"></LHeaderContent>
			</div>
		</Header>

		<div v-else
				 class="as-pattern-header"
				 :class="{'header-top-width': pattern == 2}"
				 :style="{'color' : navStyle.textColor}">
			<HNav :is-show-search="isShowSearch"
						:is-show-click-menu="isShowClickMenu"></HNav>
		</div>
		<!-- 侧边导航 控制按钮-->
		<div class="show-menu" v-if="pattern == 2">
			<transition name="fade" mode="in-out">
				<i v-if="isShowClickMenu&&!isShowSearch"
					 class="el-icon-s-fold"
					 :style="{'color' : navStyle.textColor}"
					 @click="isShowClickMenu = !isShowClickMenu"></i>
				<i v-if="!isShowClickMenu&&!isShowSearch"
					 class="el-icon-s-unfold"
					 :style="{'color' : navStyle.textColor}"
					 @click="isShowClickMenu = !isShowClickMenu"></i>
			</transition>
		</div>

		<Button class="setting"
						type="text"
						icon="el-icon-setting"
						:style="{'color' : navStyle.textColor}"
						@click="handleSetting"></Button>
	</div>
</template>
<style scoped lang="scss">
	@import "../../assets/css/variables";

	.header {
		position: relative;
		display: flex;
		flex-direction: row;
		width: 100%;
		height: 60px;
	}

	//header
	.as-header {
		padding-right: 0;
		width: calc(100% - 50px);
		font-size: $as-default-fz;
		line-height: 60px;

		.as-header-content {
			display: flex;
			height: 100%;
			font-size: $fz14;
			align-items: center;
			justify-content: flex-end;
		}
	}

	//pattern 2
	.as-pattern-header {
		width: inherit;
		font-size: $as-default-fz;
		line-height: 60px;
	}

	.setting {
		/*position: absolute;*/
		/*right: 0;*/
		width: 50px;
		/*z-index: 11;*/
	}

	.show-menu {
		position: absolute;
		left: 200px;
		top: 20px;
		z-index: 11;
		cursor: pointer;
		i{
			font-size: 24px;
		}
	}
</style>
<script>
	import HNav           from './h-nav'
	import LHeaderContent from './l-hedaer-content'
	import { resize }     from '../../utils'
	import { mapState }   from 'vuex'
	export default {
		components: { HNav, LHeaderContent },
		mixins: [],
		name: 'LHeader',
		data () {
			return {
				isShowSearch: true,
				isShowMenu: true,
				isShowClickMenu: false
			}
		},
		props: {},
		computed: {
			...mapState({
				pattern: state => state.layout.pattern,
        fixedNav: state => state.layout.fixedNav,
				fixedHeader: state => state.layout.fixedHeader,
				navStyle: state => state.layout.navStyle
			}),
			widthPx () {
				return this.pattern == 1 ? 1000 : 1300
			}
		},
		watch: {},
		created () {},
		mounted () {
			this.init()
			resize((evt) => {
				this.init()
			})
		},
		methods: {
			init () {
				this.isShowSearch = document.body.offsetWidth <= this.widthPx ? false : true
				this.isShowClickMenu = this.pattern == 2 && (document.body.offsetWidth <= this.widthPx ? false : true)
			},
      handleSetting(){
			  this.$emit('setting')
			}
		},
		filters: {}
	}
</script>