<template>
	<main class="content">
		<div class="row">
			<div class="column">
				<Header />
				<h1>Cheechie is very Hungry!</h1>
				<p class="p-body">
					Cheechie wants food but doesn't know what to make for dinner. This is very
					aggravating for Cheechie. Help her out.
				</p>
				<button class="btn--primary" @click="randomizeAllMeals">Get Dinners</button>
			</div>
			<div class="column">
				<MealCard
					v-for="(meal, i) in meals"
					:key="i"
					:meal="meal"
					@click-lock="meal.locked = !meal.locked"
					@click-takeout="meal.takeout = !meal.takeout"
					@click-change="meal.text = randomMeal(meal)"
				/>
			</div>
		</div>
	</main>
</template>

<script>
	import Header from "./Header.vue";
	import MealCard from "./MealCard.vue";
	import { dinners, takeouts } from "./dinners";
	export default {
		name: "MealGenerator",
		components: {
			Header,
			MealCard,
		},
		data() {
			return {
				dinners: dinners,
				takeouts: takeouts,
				meals: [
					{
						day: "Monday",
						text: "Chicken Noodle Soup",
						takeout: false,
						locked: false,
					},
					{
						day: "Tuesday",
						text: "Tacos",
						takeout: false,
						locked: false,
					},
					{
						day: "Wednesday",
						text: "Chicken Cutlets",
						takeout: false,
						locked: false,
					},
					{
						day: "Thursday",
						text: "Cheeseburgers",
						takeout: false,
						locked: false,
					},
					{
						day: "Friday",
						text: "Pizza",
						takeout: false,
						locked: false,
					},
					{
						day: "Saturday",
						text: "Chinese",
						takeout: false,
						locked: false,
					},
					{
						day: "Sunday",
						text: "Penne",
						takeout: false,
						locked: false,
					},
				],
			};
		},

		methods: {
			randomMeal(meal) {
				if (meal.locked) {
					return meal.text;
				} else {
					if (meal.takeout) {
						return this.getRandomMeal(takeouts);
					} else {
						return this.getRandomMeal(dinners);
					}
				}
			},
			randomizeAllMeals() {
				this.meals.forEach((meal) => {
					if (!meal.locked) {
						if (meal.takeout) {
							console.log(meal.text);
							meal.text = this.getRandomMeal(takeouts);
						} else {
							console.log(meal.text);
							meal.text = this.getRandomMeal(dinners);
						}
					}
				});
			},
			getRandomMeal(choices) {
				let randMeal = choices[Math.floor(Math.random() * choices.length)];
				console.log(randMeal);
				// if meal is already in our list
				if (this.meals.some((m) => m.text === randMeal)) {
					// try to randomize again (max of 3 times)
					let tries = 0;
					while (this.meals.some((m) => m.text === randMeal) && tries < 3) {
						randMeal = choices[Math.floor(Math.random() * choices.length)];
						tries++;
					}
					// If we couldn't set a new random meal to fallback
					if (tries >= 3) {
						randMeal = "Leftovers";
					}
				}
				return randMeal;
			},
			// Curently unused...
			// shuffle(array) {
			// 	for (let i = array.length - 1; i > 0; i--) {
			// 		let j = Math.floor(Math.random() * (i + 1)); // random index from 0 to i

			// 		// swap elements array[i] and array[j]
			// 		// we use "destructuring assignment" syntax to achieve that
			// 		// same can be written as:
			// 		// let t = array[i]; array[i] = array[j]; array[j] = t
			// 		[array[i], array[j]] = [array[j], array[i]];
			// 	}
			// },
		},
	};
</script>

<style scoped>
	h1 {
		margin-top: 0;
		font-size: 3em;
		color: #00446c;
		margin-bottom: 0.5em;
	}
	.p-body {
		margin-top: 0;
		margin-bottom: 2em;
	}
	.row {
		display: flex;
		text-align: left;
		margin-bottom: 32px;
		/* flex-wrap: wrap; */
		/* flex: 1fr 3fr; */
		/* justify-content: space-between;
		align-self: center; */
	}

	.row .column:first-child {
		margin-right: 2em;
		width: 75%;
	}

	.content {
		/* margin: 0 60px; */
		max-width: 1300px;
		display: flex;
		flex-direction: column;
		justify-content: space-evenly;
		margin: auto;
	}

	.btn--primary {
		font-family: "Poppins", sans-serif;
		cursor: pointer;
		background-color: #00a2fd;
		color: white;
		border: none;
		border-radius: 4px;
		font-size: 24px;
		font-weight: 700;
		box-shadow: 8px 8px 0 0 rgba(0, 48, 77, 0.05);
		padding: 8px 24px;
		transition: all 0.2s ease;
	}

	.btn--primary:hover {
		background-color: #0098f0;
		transform: translateX(4px) translateY(4px);
		box-shadow: none;
	}
	.column {
		width: 100%;
	}

	@media only screen and (max-width: 750px) {
		.row {
			flex-wrap: wrap;
		}
		.row .column:first-child {
			margin-right: 0;
			width: 100%;
			margin-bottom: 2em;
		}
		.card {
			margin-left: 0;
			margin-right: 0;
		}

		.content {
			margin: 0;
			transform: none;
		}
	}
	@media only screen and (max-width: 480px) {
		h1 {
			font-size: 2em;
			text-align: center;
		}
		.p-body {
			text-align: center;
		}

		.btn--primary {
			width: 100%;
			box-shadow: none;
		}

		.btn--primary:hover {
			transform: none;
		}
	}
</style>
