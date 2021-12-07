<template>
	<b-container class="container p-4 mx-0 mb-xl-0 mb-5">
		<h5 class="header mb-5">
			Filter results
		</h5>
		<b-row class="mb-4 mx-0">
			<label class="mb-1" for="name">Name Contains</label>
			<b-form-input
				id="name"
				v-model="state.name"
				type="text"
				placeholder="Search..."
			>
			</b-form-input>
		</b-row>

		<div :class="{'form-group': windowWidth < 1200 }">
			<b-row class="col-xl-12 col-sm-3 col-xs-12 mb-xl-4 mx-0 px-xl-0">
				<label class="mb-1" for="min-rating">Minimum Score</label>
				<b-form-input
					id="min-rating" 
					v-model="state.minRating" 
					type="number"
					min="1"
					max="10"
					placeholder="1-10"
				></b-form-input>
			</b-row>

			<b-row class="mx-0 col-xl-12 col-sm-7 col-xs-12 px-0">
				<label class="mb-1">Order By</label>
				<b-input-group>
					<b-button @click="toggleDirection">
						<i class="bi bi-arrow-up" v-if="state.orderDirection === 'down'"></i>
						<i class="bi bi-arrow-down" v-if="state.orderDirection === 'up'"></i>
					</b-button>
					<b-form-select v-model="state.orderingBy" :options="options"></b-form-select>
				</b-input-group>
			</b-row>
			<b-row class="mt-4 mx-0 d-flex justify-content-end col-xl-12 col-sm-2 col-xs-12">
				<b-button class="pt-1 clear" @click="clearData">
					Clear
				</b-button>
			</b-row>
		</div>
	</b-container>
</template>

<script>
export default {
	name: "Panel",
	data() {
		return {
			selected: null,
			options:[
				{ value: 'first_release_date', text: 'Release Date'},
				{ value: 'rating', text: 'Minimum Score'},
				{ value: 'name', text: 'Name'}
			],
			windowWidth: window.innerWidth,
		}
	},
	mounted() {
		window.addEventListener('resize', this.onResize);
	},
    beforeDestroy() {
        window.removeEventListener('resize', this.onResize); 
	},
	methods: {
		onResize(){
			this.windowWidth = window.innerWidth
		},
		toggleDirection(){
			const nextDirection = this.state.orderDirection === "up" ? "down" : "up"
			this.$emit("updateState", ["orderDirection", nextDirection])
		},
		clearData(){
			this.$emit("clearData");
		}
	},
	props: {
		state: {
			type: Object,
			required: true
		}
	}
}
</script>

<style scoped>
	.container {
		background-color: #0e1a2b;
		border-radius: 4px;
		max-width: unset;
	}
	.header {
		color: #ffffff;
	}

	label {
		font-size: 14px;
	}

	@media screen and (max-width: 1200px) {
		.form-group {
			display: flex;
		}

		.form-group div:first-child {
			padding-left: 0;
		}

		.form-group div:last-child {
			padding-right: 0;
		}

		button.clear {
			width: 100%;
		}
	}

	@media screen and (max-width: 575px) {
		.form-group {
			display: block;
		}

		.form-group div:first-child {
			padding-right: 0;
			margin-bottom: 1.5rem;
		}

		.form-group div:nth-child(2) {
			padding-left: 0;
			padding-right: 0;
			margin-bottom: 1.5rem;
		}

		.form-group div:last-child {
			padding-left: 0;
		}
	}
</style>