<template>
	<div class="l_main">
		<div class="l_wrapper">
			<div class="content">
				<div class="searchResult" :class="{ 'scrolled-padding': scroll }">
					<div v-if="loading">
						<h1>검색 중입니다</h1>
					</div>
					<div v-else-if="theSearch.price != ''">
						<!-- <p class="search-content">
							<span>요즘 <strong class="search-name">{{ theSearch.name }}</strong>(은)는&nbsp;</span>
							<span><strong class="search-price">&nbsp;{{ theSearch.price }}</strong><strong class="search-unit">{{ theSearch.unit }}</strong>&nbsp;입니다</span>
							<span><strong class="search-date">&nbsp;({{ theSearch.date }})</strong></span>
						</p> -->
						<p class="search-content">
							<span>요즘 <strong class="search-name">{{ theSearch.name }}</strong>&nbsp;</span>
							<span><strong class="search-unit">{{ theSearch.unit }}</strong><strong class="search-price">&nbsp;{{ theSearch.price }}</strong></span>
							<span><strong class="search-date">&nbsp;({{ theSearch.date }})</strong></span>
						</p>
						<p class="search-average">
							<span>5년 평균 : <strong class="search-price">{{ theSearch.average_price }}</strong><strong class="search-unit">{{ theSearch.unit }}</strong></span>
							<span class="resource">&lt;자료제공: 한국농수산식품유통공사(aT-Kamis)&gt;</span>
						</p>
					</div>
					<div v-else>
						<h1>가격 정보가 제공되지 않았습니다.</h1>
						<h1> 다른 과채를 검색해주세요. </h1>
					</div>
				</div>
			</div>
		</div>
	</div>
	<div class="board">
		<div class="title-wrapper">
			<h1>게시판</h1>
		</div>
		<Board :theSearch="postSearch" :fromSearch="true"/>
	</div>
</template>

<script>
import Board from './Board';
import { mapState } from 'vuex';

export default {
	name: 'Search',
	props: ['scroll'],
	components: {
		Board,
	},
	computed: {
		...mapState('product', [
			'theSearch',
			'postSearch',
			'loading'
		])
	},
	created() {
		this.$store.dispatch('post/updateMobileNav', null);
	},
}
</script>

<style lang="scss" scoped>
@import '../scss/commons.scss';
@import '../scss/main.scss';
@import '../scss/typography.scss';

.scrolled-padding {
	padding-top: 500px;
}
.content {
	.searchResult {
		// background-color: $color_prime_white;
		text-align: center;
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
		border-radius: .5em;
		padding: 1em;
		// &:before,
		// &:after {
		// 	font-size: 7em;
		// 	color: lighten($color_prime_yellow, 10%);
		// 	@media ( max-width: 500px ) {
		// 		font-size: 5em;
		// 	}
		// }
		// &:before {
		// 	content: '{';
		// }
		// &:after {
		// 	content: '}';
		// }
		h1 {
			font-size: 1.5em;
			line-height: 1.8;
			@media ( max-width: 500px ) {
				font-size: 1em;
			}
		}
		.search-content {
			padding-right: .5em;
			font-size: 1.4em;
			line-height: 1.8;
			display: inline-block;
			white-space: nowrap;
			@media ( max-width: 500px ) {
				font-size: 1em;
			}
			.search-name {
				font-size: 1.5em;
				font-family: 'Gowun Dodum', sans-serif;
				padding: 0 .2em;
				&:after {
					content: ',';
				}
				@media ( max-width: 500px ) {
					font-size: 1.2em;
				}
			}
			.search-price {
				font-size: 1.5em;
				font-family: 'Gowun Dodum', sans-serif;
				&:after {
					content: '원';
					// font-size: .5em;
				}
				@media ( max-width: 500px ) {
					font-size: 1.2em;
				}
			}
			.search-unit {
				font-size: 1.2em;
				&:after {
					content: '에';
					font-size: .8em;
				}
			}
			.search-date {
				font-size: .7em;
			}
		}
		.search-average {
			font-size: 1em;
			@media ( max-width: 500px ) {
				font-size: .8em;
			}
			.search-price {
				font-family: 'Gowun Dodum', sans-serif;
				font-weight: bold;
				&:after {
					content: '원';
					// font-size: .8em;
				}
			}
			.search-unit {
				font-size: .9em;
				&:before {
					content: '/';
				}
			}
			.resource {
				font-size: .8em;
				display: block;
				margin: .5em 0 0;
				color: gray;
			}
		}
	}
}
.board {
	margin-top: 2em;
	.title-wrapper {
		margin: 0 5em;
		text-align: center;
		position: relative;
		&:before,
		&:after {
			content: '';
			position: absolute;
			top: 50%;
			left: 0;
			width: 35%;
			border-top: 1px solid $color_prime_green;
			background: black;
			transform: translateY(-50%);
		}
		&:after {
			left: auto;
			right: 0;
		}
		h1 {
			font-size: 1.5em;
			line-height: 1.6;
		}
	}
	@media ( max-width: 500px ) {
		margin: 0;
		.title-wrapper {
			font-size: .8em;
		}
	}

}
</style>
