<template>
  	<div class="wrapper" :class="{ 'nav-open': $sidebar.showSidebar }">
    	<!-- Usado para que aparezcan notificaciones en todo el sistema -->
		<notifications></notifications>

		<!-- Menú lateral -->
		<side-bar
			:background-color="sidebarBackground"
			short-title="GL"
			title="IoTicos GL"
		>
			<template slot-scope="props" slot="links">
				<sidebar-item
					:link="{
						name: 'Dashboard',
						icon: 'tim-icons icon-chart-pie-36',
						path: '/dashboard'
					}">
				</sidebar-item>

				<sidebar-item
					:link="{
						name: 'Dispositivos',
						icon: 'tim-icons icon-mobile',
						path: '/dispositivos'
					}">
				</sidebar-item>

				<sidebar-item
					:link="{
						name: 'Alarmas',
						icon: 'tim-icons icon-volume-98',
						path: '/alarmas'
					}">
				</sidebar-item>

				<sidebar-item
					:link="{
						name: 'Plantillas',
						icon: 'tim-icons icon-paper',
						path: '/plantillas'
					}">
				</sidebar-item>
			</template>
		</side-bar>

		<!--Share plugin (for demo purposes). You can remove it if don't plan on using it-->
    	<sidebar-share :background-color.sync="sidebarBackground"></sidebar-share>

		<!-- Panel principal -->
		<div class="main-panel" :data="sidebarBackground">
			<!-- Header del sistema -->
			<dashboard-navbar></dashboard-navbar>

      		<router-view name="header"></router-view>
			<div
				:class="{ content: !isFullScreenRoute }"
				@click="toggleSidebar"
			>
				<!-- Efecto de transición -->
        		<zoom-center-transition :duration="1000" mode="out-in">
					<!-- your content here -->
					<nuxt></nuxt>
				</zoom-center-transition>
      		</div>
      		
			<!-- Pie de página -->
			<content-footer v-if="!isFullScreenRoute"></content-footer>
    	</div>
  	</div>
</template>

<script>
	/* eslint-disable no-new */
	import PerfectScrollbar from 'perfect-scrollbar'
	import 'perfect-scrollbar/css/perfect-scrollbar.css'
	import SidebarShare from '@/components/Layout/SidebarSharePlugin'
  	
	function hasElement(className) {
		return document.getElementsByClassName(className).length > 0
	}

  	function initScrollbar(className) {
    	if (hasElement(className)) {
      		new PerfectScrollbar(`.${className}`)
    	} else {
			// try to init it later in case this component is loaded async
			setTimeout(() => {
        		initScrollbar(className)
      		}, 100)
    	}
  	}

	import DashboardNavbar from '@/components/Layout/DashboardNavbar.vue'
	import ContentFooter from '@/components/Layout/ContentFooter.vue'
	import DashboardContent from '@/components/Layout/Content.vue'
	import { SlideYDownTransition, ZoomCenterTransition } from 'vue2-transitions'

  	export default {
    	components: {
			DashboardNavbar,
			ContentFooter,
			DashboardContent,
			SlideYDownTransition,
			ZoomCenterTransition,
			SidebarShare
		},
		data() {
			return {
				sidebarBackground: 'vue' //vue|blue|orange|green|red|primary
			}
		},
		computed: {
			isFullScreenRoute() {
				return this.$route.path === '/maps/full-screen'
			}
		},
		methods: {
			toggleSidebar() {
				if (this.$sidebar.showSidebar) {
					this.$sidebar.displaySidebar(false)
				}
			},
			initScrollbar() {
				let docClasses = document.body.classList
				let isWindows = navigator.platform.startsWith('Win')
				if (isWindows) {
					// if we are on windows OS we activate the perfectScrollbar function
					initScrollbar('sidebar')
					initScrollbar('main-panel')
					initScrollbar('sidebar-wrapper')

					docClasses.add('perfect-scrollbar-on')
				} else {
					docClasses.add('perfect-scrollbar-off')
				}
			}
		},
		mounted() {
			this.initScrollbar()
		}
	}
</script>

<style lang="scss">
  	$scaleSize: 0.95;
  	@keyframes zoomIn95 {
		from {
			opacity: 0;
			transform: scale3d($scaleSize, $scaleSize, $scaleSize);
		}
		to {
			opacity: 1;
		}
  	}
  	.main-panel .zoomIn {
    	animation-name: zoomIn95;
  	}

  	@keyframes zoomOut95 {
		from {
			opacity: 1;
		}
    	to {
			opacity: 0;
			transform: scale3d($scaleSize, $scaleSize, $scaleSize);
    	}
  	}
	.main-panel .zoomOut {
		animation-name: zoomOut95;
	}
</style>
