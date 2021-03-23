<script>
	export let calculatedLength;
	export let keyframes;
	export let keyframePercentages;

	let colours = ['red', 'green', 'blue', 'yellow', 'orange', 'pink'];

	$: keyframeMiddleStages = keyframes.map((keyframe, index) => {
		return `\t${keyframePercentages[index]}% {
			color: ${colours[index + 1]};
		}\n`;
	}).join(' ');

	$: styleElement = `
<style>
	.example {
		animation: cycleColours ${calculatedLength}s ease;
		animation-iteration-count: infinite;
	}

	@keyframes cycleColours {
		0% {
			color: ${colours[0]};
		}
		${keyframeMiddleStages}
		100% {
			color: black;
		}
	}
</style>
	`;
</script>

<article>
	<h4 class="example">Example Animation</h4>

	{@html styleElement}
</article>

<style>
	.thing {
		animation: name duration timing-function delay iteration-count direction fill-mode;
	}
</style>