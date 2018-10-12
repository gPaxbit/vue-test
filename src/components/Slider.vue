<template>
	<div class="slider-wrapper">
		<div class="slider-nav__left" @click="moveCarousel(-1)" :disabled="atHeadOfList"></div>
		<div class="slider" id="slider">
			<div class="slider-container">
				<div class="slider-cards" :style="{ transform: 'translateX' + '(' + currentOffset + 'px' + ')'}">
					<div :style="{width: paginationFactor + 'px'}" class="slider-card" v-for="(i, x) in this.items" :key="x">
						<div class="slider-card-inner" 
							:class="{empty: i.type == 'empty'}" 
							@mouseover="hoverHandle(i.type, $event)" 
							@mouseout="removeClone(i.type, $event)">
							<p v-if="i.type == 'empty'">Здесь может быть ваша заявка</p>
							<div v-if="i.type == 'ads'" class="slider-items">
								<div class="vip"><div v-if="i.vip"><img src="../assets/icon-star.svg" alt=""><span>vip</span></div></div>
								<span class="bordered">Рефинансирование</span>
								<span :style="{marginBottom: '20px'}">Квартира</span>
								<span>Сумма сделки</span>
								<p>123 456 р</p>
								<div class="d-none">
									<span>Стоимость недвижимости</span>
									<p>234 567 р</p>
								</div>
								<span>К/З=57%</span>
								<a href="#" class="d-none">Подробнее</a>
								<span class="xs">г. Москва, поселение Рязановское, поселок Ерино, ул. Высокая</span>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
		<div class="slider-nav__right" @click="moveCarousel(1)" :disabled="atEndOfList"></div>
	</div>
</template>

<script>
export default {
	name: 'Slider',
	data() {
		return {
			onHover: false,
			currentOffset: 0,
			windowSize: 7,
			items: [
				{id: '1', type: "empty"},
				{id: '2', type: "empty"},
				{id: '3', type: "ads"},
				{id: '4', type: "ads", vip: true},
				{id: '5', type: "ads"},
				{id: '6', type: "ads"},
				{id: '7', type: "empty"},
				{id: '8', type: "empty"},
				{id: '9', type: "empty"},
				{id: '10', type: "empty"}
			]
		}
	},
	computed: {
		paginationFactor() {
			return 1200 / this.windowSize;
		},
		atEndOfList() {
			return this.currentOffset <= (this.paginationFactor * -1) * (this.items.length - this.windowSize);
		},
		atHeadOfList() {
			return this.currentOffset > -100;
		},
	},
	mounted: function() {
		var $height;

		[...document.querySelectorAll('.slider-card-inner')].map((x) => {
			$height = x.clientHeight;
			x.style.maxHeight = $height + 'px';
		}, false);

		[...document.querySelectorAll('.slider-card')].map((y) => {
			$height = y.clientHeight;
			y.style.minHeight = $height + 'px';
		})
	},

	methods: {
		moveCarousel(direction) {
			if (direction === 1 && !this.atEndOfList) {
				this.currentOffset -= this.paginationFactor;
			} else if (direction === -1 && !this.atHeadOfList) {
				this.currentOffset += this.paginationFactor;
			}
		},

		removeClone(e) {
			var $elem;
			if(e == "ads") {
				if(!event.target.classList.contains('slider-card-inner')) {
					$elem = event.target.closest('.slider-card-inner');
				} else {
					$elem = event.target;
				} 
				
				[...$elem.querySelectorAll('.d-none')].map((x) => {
					x.style.maxHeight = '0px';
				})
				$elem.style.top = '0';
			}
		},

		hoverHandle(e) {
			var $rect;
			var $elem;
			var $e = event;
			if(!$e.target.classList.contains('slider-card-inner')) {
				$rect = $e.target.closest('.slider-card-inner').getBoundingClientRect();
				$elem = $e.target.closest('.slider-card-inner');
			} else {
				$rect = $e.target.getBoundingClientRect(); 
				$elem = $e.target;
			}
			if(e == "ads") {
				$elem.style.width = $rect.width + 'px';
				$elem.style.top = 0;
				$elem.style.left = '5px';
				$elem.style.position = 'absolute';
				$elem.style.zIndex = '99';
				[...$elem.querySelectorAll('.d-none')].map((x) => {
					x.style.maxHeight = '500px';
				})
			}	
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	$vue-teal: #42b883;
	$vue-teal-light: #42b983;

	.slider-wrapper {
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.slider {
		display: flex;
		justify-content: center;
		width: 1200px;
		&-container {
			// overflow: hidden;
			position: relative;
			max-width: 1200px;
			&:before {
				content: '';
				position: absolute;
				height: 100%;
				width: 100%;
				top: 0;
				left: -100%;
				background: #fff;
				display: inline-block;
				z-index: 99;
			}
			&:after {
				content: '';
				position: absolute;
				height: 100%;
				width: 100%;
				top: 0;
				right: -100%;
				background: #fff;
				display: inline-block;
				z-index: 99;
			}
		}
		&-nav__left,
		&-nav__right {
				display: inline-block;
				width: 15px;
				height: 15px;
				border-top: 2px solid $vue-teal;
				border-right: 2px solid $vue-teal;
				cursor: pointer;
				margin: 0 10px;
				transition: transform 150ms linear;
				flex-shrink: 0;
				position: relative;
				z-index: 101;
				&[disabled] {
					opacity: 0.2;
					border-color: black;
				}
		}
		&-nav__left {
			transform: rotate(-135deg);
			&:active {
				transform: rotate(-135deg) scale(0.9);
			}
		}
		&-nav__right {
			transform: rotate(45deg);
			&:active {
				transform: rotate(45deg) scale(0.9);
			}
		}

		&-items {
			p {
				font-weight: 500;
				font-size: 21px;
				color: #fff;
				line-height: 1;
				margin-bottom: 20px;
			}
			span {
				color: #fff;
				font-size: 13px;
				display: inline-block;
				width: 100%;
				margin-bottom: 4px;
				line-height: 1;
				&.bordered {
					text-decoration: underline;
				}
				&.xs {
					font-size: 10px;
					line-height: 1.3;
					margin-top: 15px;
				}
			}
			a {
				display: block;
				color: #fff;
				background: orange;
				text-decoration: none;
				text-align: center;
				margin: 0 -15px;
				padding: 8px 0;
				font-size: 14px;
				margin-top: 20px;
			}
			.vip {
				font-weight: 800;
				font-size: 13px;
				line-height: 1;
				margin-bottom: 10px;
				height: 18px;
				span {
					display: inline-block;
					vertical-align: middle;
					margin-left: 3px;
					width: auto;
					color: orange;
					margin-bottom: 0;
				}
				img {
					width: 14px;
					vertical-align: middle;
				}
			}
		}
	}
	.slider-cards {
		display: flex;
		transition: transform 150ms ease-out;
		transform: translatex(0px);
		
		.slider-card {
			cursor: pointer;
			z-index: 3;
			padding: 0 5px;
			flex-shrink: 0;
			width: calc(1200px/7);
			position: relative;
			&-inner {
				padding: 15px;
				outline: 2px solid orange;
				outline-offset: -2px;
				min-height: 100%;
				background: #717aab;
				font-family: sans-serif;
				transition: all .5s ease, position .1s linear .5s;
				&:not(.empty):hover {
					max-height: 500px !important;
					box-shadow: 0px 10px 25px rgba(50,100,40,.2);
					.d-none {}
					.slider-items {
						a {
							margin-top: 20px;
						}
					}
				}
				.slider-items {
					a {
						margin-top: 0;
						padding: 0;
						line-height: 40px;
					}
				}
				.d-none {
					transition: all .5s ease;
					overflow: hidden;
					max-height: 0px;
				}
				&.empty {
					outline: none;
					background: #d9ddf3;
					display: flex;
					align-items: center;
					justify-content: center;
					color: #fff;
					font-weight: 600;
					text-align: center;
					line-height: 1.2;
				}
				&.hover {
					position: absolute;
					z-index: 99;
				}
			}
		}
	}
</style>
