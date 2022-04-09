<template>
<transition name="fade" appear>

	<div
		:class="[
			'card',
			{
				'card--takeout': meal.takeout
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
		<transition name="slide-fade" mode="out-in">
			<p :key="meal.text">
				{{ meal.text }}
			</p>
		</transition>
		<div class="btn-container">
			<button class="btn-icon btn-icon--edit" @click="$emit('clickEdit')" title="Edit Meal">
				<Icon icon="fa6-solid:pencil" :width="iconSize" />
			</button>
			<button
				class="btn-icon btn-icon--change"
				@click="$emit('clickChange')"
				title="Change Meal"
				:disabled="meal.locked"
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
			delay:{
				type: Number
			}
		},
		data() {
			return {
				iconSize: 16,
			};
		},
		computed: {
			lockTitle: function () {
				return this.meal.locked ? "Unlock Meal" : "Lock Meal";
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
		box-sizing: border-box;
		transition: all 0.2s ease;
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
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .3s ease;
}
.slide-fade-enter-from, .slide-fade-leave-to {
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
	}
</style>
