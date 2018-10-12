<template>
	<div :class="{show: isToggle}" class="pane" :style="{top: '-' + this.top + 'px'}">
		<div class="slider">
			<div class="wrapper">
				<Slider></Slider>
			</div>
			<div class="title" v-if="!isToggle" @click="toggle()">VIP-лента</div>
		</div>
		<div @click="toggle()" :class="{showIcon: isToggle}" class="toggle"></div>
	</div>
</template>

<script>
import Slider from './Slider.vue';

export default {
	name: 'Pane',
	components: {
		Slider
	},
	mounted () {
		this.top = document.querySelector('.wrapper').clientHeight + 15;
	},
	data () {
		return {
			isToggle: false,
			top: 0,
		}
	},
	methods: {
		toggle () {
			this.isToggle = !this.isToggle;
			return false;
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	.slider {
		position: relative;
		z-index: 9;
	}
	.pane {
		position: absolute;
		top: 0;
		width: 100%;
		left: 0;
		padding: 15px;
		background: #fff;
		box-shadow: 0px 15px 30px rgba(120,150,120,0.1);
		transition: all .6s ease;
		top: -400px;

		&.show {
			top: 0 !important;
		}

		.title {
			text-align: center;
			font-size: 14px;
			color: orange;
			font-weight: 600;
			font-family: sans-serif;
			line-height: 55px;
			height: 40px;
		}
		.wrapper {
			display: block;
			width: 100%;
			max-width: 1170px;
			padding: 0 15px;
			margin: 0 auto;
		}
	}

	.toggle {
		background: #fff url(../assets/down-arrow.png) center / 20px auto no-repeat;
		cursor: pointer;
		display: inline-block;
		position: absolute;
		width: 48px;
		height: 28px;
		bottom: -28px;
		left: 50%;
		margin-left: -24px;
		box-shadow: 0px 4px 6px rgba(100,100,100,0.2);
		&.showIcon {
			transform: rotate(180deg);
			box-shadow: 0px -4px 6px rgba(100,100,100,0.2);
		}
	}
</style>
