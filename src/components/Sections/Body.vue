<template>
	<b-container class="mt-xl-0 mt-5 px-0 pl-xl-4">
		<Card v-for="(game,index) in games" :key="index" :index="index" :game="game"/>
	</b-container>
</template>

<script>
import Card from "@/components/Sections/Card";
export default {
	name: "Body",
	components: { Card },
	props: {
		games: {
			type: Array,
			required: true
		}
	}
}
</script>

<style scoped>
  .container {
    max-width: unset;
  }
</style>