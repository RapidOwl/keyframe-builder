<script>
	let animationName = 'my-animation';

	let stages = [
		{ name: 'Start', length: 3 },
		{ name: 'Second', length: 5 },
		{ name: 'Third', length: 10 },
	];
	$: calculatedLength = stages.reduce((a, b) => a + (b['length'] || 0), 0);
	$: keyframeDeclarations = stages.map((stage) => {
		return `\t${calculatePercentage(stage.length)}%\n\t{\n\t}\n`;
	});

	let newStageName = '';
	let newStageLength = 0;

	function addStage() {
		if (newStageName.length > 0) {
			stages = [...stages, { name: newStageName, length: newStageLength }];
			newStageName = '';
		}
	}

	function removeStage(i) {
		stages = stages.filter((stages, index) => {
			return i !== index;
		});
	}

	function calculatePercentage(length) {
		return Math.round((length / calculatedLength) * 100);
	}
</script>

<header>
	<h1>Keyframe Builder</h1>
</header>
<main>
	<section>
		<label for="AnimationName">Animation Name</label>
		<input id="AnimationName" type="text" bind:value={animationName} />

		<p>(Total Length: {calculatedLength} seconds)</p>
		{#each stages as { name, length }, i}
			<h2>{name} ({calculatePercentage(length)}%)</h2>
			<label for="StageLength[{i}]">Length</label>
			<input type="number" id="StageLength[{i}]" bind:value={length} />
			{#if i !== 0}
				<button on:click={() => removeStage(i)}>❌</button>
			{/if}
		{/each}
		<h2>End (100%)</h2>
	</section>
	<hr />
	<section>
		<input type="text" bind:value={newStageName} />
		<input type="number" bind:value={newStageLength} />
		<button on:click={addStage} disabled={newStageName.length === 0}>Add Stage</button>
	</section>
	<!-- <pre>{JSON.stringify(stages)}</pre> -->
	<pre>
@keyframes {animationName} &#123;
	0% &#123;
	&#125;
{#each keyframeDeclarations as declaration}
	{declaration}
{/each}&#9;100% &#123;
	&#125;
&#125;
	</pre>
</main>

<style>
</style>
