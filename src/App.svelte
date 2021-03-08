<script>
	let animationName = 'my-animation';

	// TODO Is this the best way to model this?
	let stages = [
		{ name: 'Start', length: 3 },
		{ name: 'Second', length: 5 },
		{ name: 'Third', length: 10 },
	];
	$: calculatedLength = stages.reduce((a, b) => a + (b['length'] || 0), 0);
	$: keyframeDeclarations = stages.map((stage) => {
		return `\t${calculatePercentage(stage.length)}% {\n\t}\n`;
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
		<article>
			<label for="AnimationName">Animation Name</label>
			<input id="AnimationName" type="text" bind:value={animationName} />
		</article>

		<article>
			<h2>Keyframes</h2>
			<h4>0%</h4>
			{#each stages as { name, length }, i}
				<h4>{calculatePercentage(length)}%</h4>
				<label for="StageLength[{i}]">Length</label>
				<input type="number" id="StageLength[{i}]" bind:value={length} />
				{#if i !== 0}
					<button on:click={() => removeStage(i)} aria-label="Remove animation stage"
						>❌</button
					>
				{/if}
			{/each}
			<h4>End (100%)</h4>
		</article>

		<article>
			<h4>Add a new keyframe</h4>
			<!-- <input type="text" bind:value={newStageName} /> -->
			<label for="NewStageLength">Seconds until this keyframe</label>
			<input type="number" bind:value={newStageLength} />
			<button on:click={addStage} disabled={newStageName.length === 0}>Add keyframe</button>
		</article>
	</section>
	<!-- <pre>{JSON.stringify(stages)}</pre> -->
	<section>
		<pre>
.animated-element &#123;
	animation: {animationName} {calculatedLength}s;
&#125;

@keyframes {animationName} &#123;
	0% &#123;
	&#125;
{#each keyframeDeclarations as declaration}
	{declaration}
{/each}&#9;100% &#123;
	&#125;
&#125;
	</pre>
	</section>
</main>

<style>
	main {
		display: flex;
		flex-direction: row;
	}

	pre {
		tab-size: 4;
	}

	article {
		margin: 0 24px 24px 0;
	}

	article,
	pre {
		padding: 16px;
		border-radius: 16px;
		box-shadow: 0 1px 1px rgba(0, 0, 0, 0.12), 0 2px 2px rgba(0, 0, 0, 0.12),
			0 4px 4px rgba(0, 0, 0, 0.12), 0 8px 8px rgba(0, 0, 0, 0.12),
			0 16px 16px rgba(0, 0, 0, 0.12);
	}
</style>
