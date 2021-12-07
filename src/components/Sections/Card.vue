<template>
	<div class="container mb-3 mx-0">
		<b-row>
			<b-col class="col-sm-2 col-12 spacer d-flex justify-content-end pt-3">
				<span class="rating d-flex justify-content-center align-items-center" v-if="windowWidth < 576">{{ rating }}</span>
			</b-col>
			<b-col class="col-sm-9 col-12 mt-3 mb-2">
				<h4 class="mb-0">{{ game.name }}</h4>
				<p class="mb-3">Release Date: {{ first_release_date }}</p>
				<p>{{ game.summary }}</p>
			</b-col>
			<b-col class="col-1 d-flex justify-content-center align-items-center" v-if="windowWidth >= 576">
				<span class="rating d-flex justify-content-center align-items-center">{{ rating }}</span>
			</b-col>
		</b-row>
	</div>
</template>

<script>
import moment from 'moment'
export default {
	name: "Card",
	computed: {
		first_release_date(){
			return moment(this.game.first_release_date).format('DD/MM/YYYY')
		},
		rating(){
			return Math.ceil(this.game.rating / 10)
		} 	
	},
	props: {
		game: {
			type: Object
		}
	},
	data() {
		return {
			windowWidth: window.innerWidth,
		}
	},
	mounted() {
		window.addEventListener('resize', this.onResize);
	},
	methods: {
		onResize(){
			this.windowWidth = window.innerWidth
		}
	}
}
</script>

<style scoped>
.container {
	background-color: #0e1a2b;
	width: 100%;
	position: relative;
	border-radius: 4px;
	overflow: hidden;
	border: none;
}

.spacer {
	background: #000000;
	min-height: 150px;
}

h6 {
	color: #c1d1e8 !important;
}

h4 {
	font-size: 1.5rem;
	font-weight: bold;
}

p {
	font-size: 14px;
	color: #c1d1e8 !important;
	display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    text-overflow: ellipsis;
    overflow: hidden; 

}

.rating {
	min-width: 2.5rem;
	min-height: 2.5rem;
	max-width: 2.5rem;
	max-height: 2.5rem;
	border-radius: 50%;
	background: #5692e8;
	color: white;
	font-size: 1.6rem;
}

@media screen and (max-width: 575px) {
	.rating {
		min-width: 1.8rem;
		min-height: 1.8rem;
		max-width: 1.8rem;
		max-height: 1.8rem;
		font-size: 1.3rem;
	}
}

</style>