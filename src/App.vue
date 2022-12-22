<script setup>
	import Progress from "./components/Progress.vue";
	import Overlay from "./components/Overlay.vue";
	import { ref } from "vue";

	const inputHandle = ref("");
	const inputButton = ref(null);
	const answerIsWrong = ref("");
	const gameOver = {
		toggle: ref(false),
		msg: ref(""),
	};
	let stats = {
		score: ref(0),
		mistakes: ref(0),
		currentProblem: ref(""),
	};

	const resetButton = ref(null);

	function test() {
		alert("test function moment");
	}

	function generateNumber(max) {
		return Math.floor(Math.random() * (max + 1));
	}

	function generateProblem() {
		stats.currentProblem.value = {
			numberOne: generateNumber(10),
			numberTwo: generateNumber(10),
			operator: ["+", "-", "x"][generateNumber(2)],
		};
	}

	function resultHandle() {
		if (stats.score.value === 10) {
			gameOver.msg.value = "yay ya won!";
			gameOver.toggle.value = true;
		}
		if (stats.mistakes.value === 3) {
			gameOver.msg.value = "ain't no way you lost";
			gameOver.toggle.value = true;
		}
	}

	function restartGame() {
		stats.score.value = 0;
		stats.mistakes.value = 0;
		stats.currentProblem.value = "";
		gameOver.toggle.value = false;
		inputHandle.value = "";
		inputButton.value.focus();
		generateProblem();
	}

	function submitHandle() {
		let correctAnswer;
		const p = stats.currentProblem.value;

		// calculates correct answer
		if (p.operator === "+") correctAnswer = p.numberOne + p.numberTwo;
		if (p.operator === "-") correctAnswer = p.numberOne - p.numberTwo;
		if (p.operator === "x") correctAnswer = p.numberOne * p.numberTwo;

		// handles answer input
		if (correctAnswer === parseInt(inputHandle.value)) {
			stats.score.value++;
			generateProblem();
		} else {
			stats.mistakes.value++;
			answerIsWrong.value = "animate-wrong";
			setTimeout(() => (answerIsWrong.value = ""), 401);
		}

		// clears input & refocuses on it
		inputHandle.value = "";
		inputButton.value.focus();

		// handles the result
		resultHandle();
	}

	generateProblem();
</script>

<template>
	<p class="problem" :class="answerIsWrong">
		{{ stats.currentProblem.value.numberOne }}
		{{ stats.currentProblem.value.operator }}
		{{ stats.currentProblem.value.numberTwo }}
	</p>

	<form action="" class="our-form" @submit.prevent="submitHandle">
		<input
			v-model.trim="inputHandle"
			type="text"
			class="our-field"
			autocomplete="off"
			ref="inputButton"
		/>
		<button>Submit</button>
	</form>

	<p class="status">
		You need {{ 10 - stats.score.value }} more points, and are allowed to
		make {{ 2 - stats.mistakes.value }} more mistakes.
	</p>

	<Progress :progress="stats.score.value / 10" />

	<Overlay
		v-if="gameOver.toggle.value === true"
		:msg="gameOver.msg.value"
		:function="restartGame"
	/>

	<div class="footer">
		<a target="_blank" href="https://youtu.be/EVze4Cq-dZ8">
			== Brad's Bootcamp tutorial video ==
		</a>
	</div>
</template>
