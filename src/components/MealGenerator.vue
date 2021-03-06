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
				<button class="btn--share" @click="share">Share</button>
			</div>
			<div class="column">
				<MealCard
					v-for="(meal, i) in meals"
					:key="i"
					:meal="meal"
					@click-lock="meal.locked = !meal.locked"
					@click-takeout="meal.takeout = !meal.takeout"
					@click-change="meal.text = randomMeal(meal)"
					@click-edit="clickEdit(meal)"
					@click-save="clickSave($event, meal)"
					@click-cancel="clickCancel(meal)"
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
						text: "",
						takeout: false,
						locked: false,
						editing: false,
					},
					{
						day: "Tuesday",
						text: "",
						takeout: false,
						locked: false,
						editing: false,
					},
					{
						day: "Wednesday",
						text: "",
						takeout: false,
						locked: false,
						editing: false,
					},
					{
						day: "Thursday",
						text: "",
						takeout: false,
						locked: false,
						editing: false,
					},
					{
						day: "Friday",
						text: "",
						takeout: false,
						locked: false,
						editing: false,
					},
					{
						day: "Saturday",
						text: "",
						takeout: false,
						locked: false,
						editing: false,
					},
					{
						day: "Sunday",
						text: "",
						takeout: false,
						locked: false,
						editing: false,
					},
				],
			};
		},
		computed: {
			cardDelay: function (index) {
				return `transition-delay: ${0.2 * index}s`;
			},
		},
		created() {
			this.randomizeAllMeals();
		},
		methods: {
			randomMeal(meal) {
				// change an individual meal unless its locked.
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
				// generate a random meal for every day of the week
				this.meals.forEach((meal) => {
					if (!meal.locked) {
						if (meal.takeout) {
							// choose meal from takeout menu
							meal.text = this.getRandomMeal(takeouts);
						} else {
							meal.text = this.getRandomMeal(dinners);
						}
					}
				});
			},
			getRandomMeal(choices) {
				let randMeal = choices[Math.floor(Math.random() * choices.length)];
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
			clickSave($event, meal) {
				// save meal using input text
				meal.text = $event;
				meal.editing = false;
			},
			clickEdit(meal) {
				meal.editing = !meal.editing;
			},
			clickCancel(meal) {
				meal.editing = false;
			},
			share() {
				let str = `This week, Cheechie was thinking...\n\n`;
				this.meals.forEach((meal) => {
					str += meal.text + "\n";
				});

				// copy summary string to clipboard, for easy copy and paste
				navigator.clipboard.writeText(str);
				this.$toast.show(`Copied to Clipboard`, { duration: 2000 });
			},
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
	}

	.row .column:first-child {
		margin-right: 2em;
		width: 75%;
	}

	.content {
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
		border: 2px solid #00a2fd;
		border-radius: 4px;
		font-size: 24px;
		font-weight: 700;
		box-shadow: 8px 8px 0 0 rgba(0, 48, 77, 0.05);
		padding: 8px 24px;
		transition: all 0.2s ease;
	}
	.btn--share {
		display: block;
		margin: 1em 0;
		font-family: "Poppins", sans-serif;
		cursor: pointer;
		background-color: transparent;
		color: #00a2fd;
		border: 2px solid #00a2fd;
		border-radius: 4px;
		font-size: 18px;
		font-weight: 700;
		padding: 8px 24px;
		transition: all 0.2s ease;
	}

	.btn--primary:hover {
		border: 2px solid #0098f0;
		background-color: #0098f0;
		transform: translateX(4px) translateY(4px);
		box-shadow: none;
	}

	.btn--share:hover {
		background-color: rgba(0, 152, 240, 0.1);
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
		.btn--share,
		.btn--primary {
			width: 100%;
			box-shadow: none;
		}

		.btn--primary:hover {
			transform: none;
		}
	}
</style>
