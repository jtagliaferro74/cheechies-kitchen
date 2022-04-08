<template>
	<MealGenerator />
	<!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
	<!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
</template>

<script>
	// import HelloWorld from "./components/HelloWorld.vue";

	import MealGenerator from "./components/MealGenerator.vue";

	export default {
		name: "App",
		components: {
			// HelloWorld,

			MealGenerator,
		},
	};
</script>

<style>
	@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;700&display=swap");
	html,
	body {
		margin: 0;
		background-color: #e3f2fb;
	}

	#app {
		font-family: "Poppins", sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #00446c;
		margin-top: 60px;
		margin-left: 60px;
		margin-right: 60px;
	}

	@media only screen and (max-width: 750px) {
		#app {
			margin-top: 30px;
			margin-left: 30px;
			margin-right: 30px;
		}
	}
</style>
