<template>
	<a-layout-sider v-model="collapsed" :trigger="false" :style="layoutSiderStyle" class="layout__sider-bar">
		<div class="sider-bar__content">
			<div class="logo">{{appName}}</div>
			<sh-menu :menu="sideMenuRoutes" :collapsed="collapsed"></sh-menu>
		</div>
	</a-layout-sider>
</template>

<script>
import ShMenu from './menu'
import { deepCopy } from '@gauseen/tools'
import { arr2Tree } from '@/utils/arr2Tree'

export default {
	name: 'SiderBar',
	props: {
		currentPath: {
			type: String,
		},
	},
	data () {
		return {
			sideMenuRoutes: [],
		}
	},
	components: {
		ShMenu,
	},
	computed: {
		collapsed () {
			return this.$store.state.collapsed
		},
		appName () {
			return this.collapsed ? 'admin' : '后台管理中心'
		},
		// 获取侧边菜单导航
		dynamicSideMenus () {
			return deepCopy(this.$store.state.dynamicMenus).sort((a, b) => a.menu_sort - b.menu_sort)
		},
		layoutSiderStyle () {
			return { overflow: 'auto', height: '100vh', position: 'fixed', left: 0 }
		},
	},
	created () {
		// 生成侧边导航栏
		this.generateSideRoutes()
	},
	watch: {
	},
	methods: {
		generateSideRoutes () {
			let sideMenusOfFlat = this.dynamicSideMenus.map(item => {
				return {
					// frontUrl: item.front_url,
					// 路由跳转
					path: item.right_action,
					meta: {
						icon: item.menu_icon,
						title: item.right_name,
					},
					menu_sort: item.menu_sort,
					pId: item.parent_id,
					id: item.right_id,
				}
			})
			this.sideMenuRoutes = arr2Tree(sideMenusOfFlat)
			// console.log('this.sideMenuRoutes: ', this.sideMenuRoutes)
		},
	},
}
</script>

<style>
.ant-layout-sider {
	background: #333a4e;
}
.ant-menu-dark,
.ant-menu-dark .ant-menu-sub
{
	background: #333a4e !important;
	box-shadow: none !important;
}
</style>

<style scoped>
.layout__sider-bar {
	height: 100vh;
	overflow: hidden;
	background-color: #333a4e;
}
.layout__sider-bar >>> .logo {
	height: 60px;
	line-height: 60px;
	font-size: 18px;
	text-align: center;
	color: #fff;
	background-color: #333a4e;
}

.layout__sider-bar >>> .ant-menu-sub.ant-menu-inline > .ant-menu-item,
.layout__sider-bar >>> .ant-menu-inline .ant-menu-submenu-title
{
	font-size: 14px;
	height: 50px !important;
	line-height: 50px !important;
	margin: 8px 0;
	background-color: #333a4e;
}
.layout__sider-bar >>> .ant-menu-inline .ant-menu-item.ant-menu-item-selected {
	background-color: #108ee9;
}
.layout__sider-bar >>> .ant-menu-inline .ant-menu-submenu-inline.ant-menu-submenu-open > .ant-menu-submenu-title
{
	border-left: 3px solid #4a90e2;
	background-color: #3e445c;
}
</style>
