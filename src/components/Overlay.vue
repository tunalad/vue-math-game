<script setup>
	const props = defineProps(["msg", "function"]);
</script>

<template>
	<div class="overlay overlay--visible">
		<div class="overlay-inner">
			<p class="end-message">{{ props.msg }}</p>
			<button class="reset-button" @click="props.function">
				Start Over
			</button>
		</div>
	</div>
</template>
