<script setup>
	import { ref } from "vue";

	const props = defineProps(["progress"]);
</script>

<template>
	<div class="progress">
		<div class="boxes">
			<div class="box"></div>
			<div class="box"></div>
			<div class="box"></div>
			<div class="box"></div>
			<div class="box"></div>
			<div class="box"></div>
			<div class="box"></div>
			<div class="box"></div>
			<div class="box"></div>
			<div class="box"></div>
		</div>
		<div
			class="progress-inner"
			:style="{ transform: 'scaleX(' + props.progress + ')' }"
		></div>
	</div>
</template>

<style>
	.progress {
		border: 1px solid #c7c7c7;
		border-right: none;
		position: relative;
	}

	.progress-inner {
		position: absolute;
		top: 0;
		bottom: 0;
		width: 100%;
		background-color: #7ecc00;
		opacity: 0.57;
		transform: scaleX(0.1);
		transform-origin: center left;
		transition: transform 0.4s ease-out;
	}
</style>
