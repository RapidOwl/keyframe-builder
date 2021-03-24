<script>
	export let calculatedLength;
	export let keyframes;
	export let keyframePercentages;

	let colours = ['red', 'green', 'blue', 'yellow', 'orange', 'pink'];

	$: keyframeMiddleStages = keyframes.map((keyframe, index) => {
		return `\t${keyframePercentages[index]}% {
			background: ${colours[index]};
			width: ${keyframePercentages[index]}%;
			opacity: 1;
		}\n`;
	}).join(' ');

	$: styleElement = `
<style>
	.example-core {
		animation: cycleColours ${calculatedLength}s ease;
		animation-iteration-count: infinite;
		opacity: 0;
	}

	@keyframes cycleColours {
		0% {
			background: black;
			width: 28px;
			opacity: 0;
		}
		${keyframeMiddleStages}
		100% {
			background: black;
			opacity: 0;
			width: 100%;
		}
	}
</style>
	`;
</script>

<article>
	{@html styleElement}

	<div class="example-wrapper">
		<div class="example-core"></div>
	</div>
</article>

<style>
	.example-wrapper {
		height: 40px;
		border: 3px solid black;
		border-radius: 23px;
		padding: 3px;
		margin-bottom: 24px;
	}

	@media (prefers-reduced-motion) {
		.example-wrapper {
			clip: rect(0 0 0 0);
			clip-path: inset(50%);
			height: 1px;
			overflow: hidden;
			position: absolute;
			white-space: nowrap;
			width: 1px;
		}
	}

	.example-core {
		height: 28px;
		width: 28px;
		border-radius: 14px;
	}
</style>