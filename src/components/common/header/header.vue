<template>
	<header class="header" v-if="!store.isSmallPage">
		<gal-logo class="logo"></gal-logo>
		<gal-nav class="nav"></gal-nav>
		<gal-icon-button
			v-for="(item, index) in headerIconList"
			:key="index"
			class="headerIcon"
			:icon="item.icon"
			circle
			v-gal-tooltip="item.text"
			@click="item.click"
		></gal-icon-button>
	</header>
	<header class="header" v-else>
		<gal-icon-button
			class="small-header-icon"
			theme="solid"
			icon="menu"
		></gal-icon-button>
		<h1 class="title">CnGal 中文GalGame资料站</h1>
		<gal-icon-button
			class="small-header-icon"
			icon="search"
			theme="solid"
			v-gal-tooltip="'搜索'"
			role="link"
			@click="router.push('/search')"
		></gal-icon-button>
	</header>

	<gal-dialog ref="settingDialog" :isModal="true">
		<gal-card class="setting-dialog">
			<h2 class="dialog-title">主题设置</h2>
			<h3>主题颜色</h3>
			<p>
				你可以从下方选择一个喜欢的颜色作为主题颜色，选择黑色可以进入夜间模式
			</p>
			<div>
				<gal-button
					v-for="item in colorList"
					:key="item"
					class="color"
					:style="{
						'--button-bg-color': item,
						'--button-hover-bg-color': item,
						'--button-border-radius': 'unset'
					}"
					theme="solid"
					@click="changeTheme(item)"
				></gal-button>
			</div>
			<h3>背景图</h3>
			<p>
				显示背景图会将卡片设置为半透明状态，目前建议使用明亮的图片作为背景
			</p>
			<div>
				<galCheckbox
					v-model="isTransparent"
					@change="changeTr"
					label="是否显示背景图片"
				></galCheckbox>
			</div>
		</gal-card>
	</gal-dialog>
</template>

<script setup>
import { ref, onMounted } from "vue";
import galLogo from "../logo/logo.vue";
import galNav from "../nav/nav.vue";
import { useStore } from "@/store/index.js";

import { useRouter } from "vue-router";
const router = useRouter();
const store = useStore();

const settingDialog = ref(null);
const headerIconList = ref([
	{
		icon: "search",
		text: "搜索",
		click() {
			router.push("/search");
		}
	},
	{
		icon: "login",
		text: "登陆",
		click() {
			router.push("/account/login");
		}
	},
	{
		icon: "settings",
		text: "设置",
		click() {
			settingDialog.value.show();
		}
	}
]);

const colorList = [
	"#f44336",
	"#f06292",
	"#9c27b0",
	"#673ab7",
	"#3f51b5",
	"#2196f3",
	"#03a9f4",
	"#00bcd4",
	"#009688",
	"#4caf50",
	"#8bc34a",
	"#cddc39",
	"#ffeb3b",
	"#ffc107",
	"#ff9800",
	"#ff5722",
	"#795548",
	"#607d8b",
	"#000000"
];
const changeTheme = (theme) => {
	store.changeTheme({
		color: theme
	});
};

const isTransparent = ref(store.theme.isTransparent);

const changeTr = () => {
	store.changeTheme({
		isTransparent: isTransparent.value
	});
};
</script>

<style scoped>
.header {
	--header-height: 64px;
	--header-bg-color: #ffffffaa;
	--header-bg-color-dark: var(--main-bg-color);
	--header-font-color: var(--gray-color);
}

.theme-dark .header {
	--header-bg-color: var(--header-bg-color-dark);
}

@media screen and (max-width: 768px) {
	.header {
		--header-height: 48px;
		--header-bg-color: var(--main-color);
		--header-font-color: var(--white-color);
	}
}

.header {
	height: var(--header-height);
	position: sticky;
	top: 0;
	display: flex;
	align-items: center;
	box-shadow: var(--main-shadow);
	color: var(--header-font-color);
	padding: 0 16px;
	z-index: 9;
	background-color: var(--header-bg-color);
	backdrop-filter: blur(5px);
}
.logo {
	margin-inline-start: 16px;
}
.nav {
	margin-inline-start: 32px;
	flex: 1;
}
.headerIcon {
	margin-inline-end: 1em;
	font-size: 24px;
}

.title {
	font-size: large;
	flex: 1;
	font-weight: normal;
}

.setting-dialog {
	width: 440px;
	padding: 24px;
}
.dialog-title {
	color: var(--main-color);
}
.theme-dark .dialog-title {
	color: var(--main-font-color);
}
.color {
	display: inline-flex;
	border: none;
	width: 28px;
	height: 28px;
	vertical-align: bottom;
}

.small-header-icon {
	font-size: 24px;
}
</style>
