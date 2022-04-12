<template>
	<transition name="fade" appear>
		<div
			:class="[
				'card',
				{
					'card--takeout': meal.takeout,
				},
			]"
		>
			<div class="card-heading">
				<h3>
					{{ meal.day }}
				</h3>
				<div class="card-heading_btn-container">
					<button
						:class="[
							'btn-icon',
							'btn-icon--tag',
							{
								'btn-icon--tag--active': meal.takeout,
							},
						]"
						@click="$emit('clickTakeout')"
						title="Set as Takeout"
					>
						<Icon icon="fa6-solid:tag" :width="iconSize" />
					</button>
					<button
						:class="[
							'btn-icon',
							'btn-icon--lock',
							{
								'btn-icon--lock--active': meal.locked,
							},
						]"
						@click="$emit('clickLock')"
						:title="lockTitle"
					>
						<Icon v-if="meal.locked" icon="fa6-solid:lock" :width="iconSize" />
						<Icon v-else icon="fa6-solid:unlock" :width="iconSize" />
					</button>
				</div>
			</div>
			<transition name="slide-fade" mode="out-in" v-if="!meal.editing">
				<p :key="meal.text">
					{{ meal.text }}
				</p>
			</transition>
			<transition name="slide-fade" mode="out-in" v-else>
				<div>
					<input
						ref="inputBox"
						@keyup.enter="clickSave(inputValue)"
						@keyup.esc="clickCancel"
						type="text"
						v-model="inputValue"
					/>
					<div class="button-container--input">
						<button class="btn--save" @click="clickSave(inputValue)">Save</button>
						<button class="btn--cancel" @click="clickCancel">Cancel</button>
					</div>
				</div>
			</transition>
			<div class="btn-container">
				<button
					v-if="meal.editing"
					class="btn-icon btn-icon--edit"
					@click="clickCancel"
					title="Cancel"
				>
					<Icon icon="fa6-solid:xmark" :width="iconSize" />
				</button>
				<button v-else class="btn-icon btn-icon--edit" @click="clickEdit" title="Edit Meal">
					<Icon icon="fa6-solid:pencil" :width="iconSize" />
				</button>
				<button
					class="btn-icon btn-icon--change"
					@click="$emit('clickChange')"
					title="Change Meal"
					:disabled="meal.locked || meal.editing"
				>
					<Icon icon="fa6-solid:rotate" :width="iconSize" />
				</button>
			</div>
		</div>
	</transition>
</template>

<script>
	import { Icon } from "@iconify/vue";
	export default {
		name: "MealCard",
		components: {
			Icon,
		},
		props: {
			meal: {
				required: true,
			},
			delay: {
				type: Number,
			},
		},
		data() {
			return {
				iconSize: 16,
				inputValue: this.meal.text,
				// editing: false,
			};
		},
		computed: {
			lockTitle: function () {
				return this.meal.locked ? "Unlock Meal" : "Lock Meal";
			},
		},
		methods: {
			clickEdit() {
				this.inputValue = this.meal.text;
				this.$emit("clickEdit", this.inputValue);
				// executes after clickEdit in parent
				if (this.meal.editing) {
					// focus input
					this.$nextTick(() => {
						this.$refs.inputBox.focus();
						console.log();
					});
				}
			},
			clickSave(input) {
				if (input === "") {
					input = this.meal.text;
				}
				this.$emit("clickSave", input);
			},
			clickCancel() {
				this.$emit("clickCancel");
			},
		},
	};
</script>

<style scoped>
	button {
		cursor: pointer;
	}
	.card {
		width: 95%;
		background-color: #fff;
		color: black;
		padding: 8px;
		margin: auto;
		margin-bottom: 16px;
		border-radius: 8px;
		box-shadow: 8px 8px 0 0 rgba(0, 48, 77, 0.05);
		/* box-sizing: border-box; */
		transition: all 0.2s ease;
		word-wrap: break-word;
	}

	.card--takeout {
		/* background-color: rgb(254, 255, 212); */
		border: 3px solid #ffb039;
		/* transition: none; */
		transition: all 0.2s ease;
	}

	.card:hover {
		transform: scale(1.03);
	}

	.card-heading {
		display: flex;
		justify-content: space-between;
	}
	.card-heading_btn-container {
		display: flex;
		justify-content: flex-start;
		align-items: center;
	}
	h3 {
		margin: 0;
		font-weight: normal;
		font-size: 14px;
		color: #666;
	}
	p {
		font-weight: bold;
		font-size: 18px;
		margin: 0;
	}
	.btn-container {
		/* margin-top: 1em; */
		display: flex;
		justify-content: flex-end;
	}

	.btn-container--input {
		display: flex;
	}

	.btn-icon {
		border: none;
		background-color: transparent;
		border-radius: 4px;
		/* height: 28px;
		width: 28px; */
		transition: 0.2s ease;
		color: rgb(199, 199, 199);
		display: flex;
		align-items: center;
		padding: 6px;
		margin: 0 5px;
	}

	.btn--save {
		margin-top: 0.5em;
		margin-right: 0.5em;
		background-color: #0098f0;
		background-color: #00a2fd;
		color: white;
		border-radius: 3px;
		padding: 2px 12px;
		border: 1px solid #00a2fd;
		transition: all 0.2s ease;
	}
	.btn--save:hover {
		border: 1px solid #0098f0;
		background-color: #0098f0;
	}

	.btn--cancel {
		margin-top: 0.5em;
		margin-right: 0.5em;
		background-color: transparent;
		color: #0098f0;
		color: #00a2fd;
		border-radius: 3px;
		padding: 2px 12px;
		border: 1px solid #00a2fd;
		transition: all 0.2s ease;
	}

	.btn--cancel:hover {
		background-color: rgba(0, 152, 240, 0.1);
	}

	.btn-icon:disabled {
		color: rgb(224, 224, 224);
		pointer-events: none;
	}

	.btn-icon--tag--active {
		color: #ffb039;
		/* padding: 4px; */
	}
	.btn-icon--tag:hover {
		background-color: #ffb039;
	}

	.btn-icon--edit {
		color: #00a2fd;
	}
	.btn-icon--edit:hover {
		background-color: #00a2fd;
	}
	.btn-icon--lock--active {
		color: #5a5a5a;
	}
	.btn-icon--lock:hover {
		background-color: #878787;
	}
	.btn-icon--change {
		color: #00d287;
	}
	.btn-icon--change:hover {
		background-color: #00d287;
	}

	.btn-icon:hover {
		border: none;
		color: #fff;
		transform: scale(1.2);
	}
	button,
	input {
		font-family: "Poppins", sans-serif;
		font-weight: bold;
	}
	input {
		max-width: 100%;
	}
	.fade-enter-active,
	.fade-leave-active {
		transition: all 0.5s ease;
	}

	.fade-enter-from,
	.fade-leave-to {
		opacity: 0;
		transform: scale(0);
	}

	.slide-fade-enter-active {
		transition: all 0.3s ease;
	}
	.slide-fade-leave-active {
		transition: all 0.3s ease;
	}
	.slide-fade-enter-from,
	.slide-fade-leave-to {
		transform: translateX(20px);
		opacity: 0;
	}

	@media only screen and (max-width: 768px) {
		.btn-icon svg {
			height: 20px;
			width: 20px;
		}

		.card:hover {
			transform: scale(1);
		}

		p {
			margin-top: 1em;
			margin-bottom: 1em;
		}
	}
</style>
