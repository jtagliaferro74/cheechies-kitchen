<template>
	<header>
		<div class="brand">
			<span class="brand_wordmark"> Cheechie's Kitchen </span>
			<Icon class="brand_icon" icon="fa6-solid:spoon" />
		</div>
	</header>
</template>

<script>
	import { Icon } from "@iconify/vue";
	export default {
		components: {
			Icon,
		},
	};
</script>

<style scoped>
	@import url("https://fonts.googleapis.com/css2?family=Amatic+SC:wght@700&display=swap");
	header {
		/* font-family: "Amatic SC", cursive; */
		display: flex;
		justify-content: flex-start;
		margin-bottom: 2em;
	}
	.brand {
		display: flex;
		justify-content: center;
		align-items: center;
		/* font-weight: bold; */
		color: hsl(202, 100%, 21%);
	}
	.brand_wordmark {
		font-size: 20px;
	}
	svg.brand_icon {
		padding: 0 4px;
	}

	@media only screen and (max-width: 480px) {
		.brand_wordmark {
			font-size: 14px;
		}
		header {
			margin-bottom: 1em;
		}
	}
</style>
