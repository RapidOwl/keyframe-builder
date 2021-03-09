<script>
	let animationName = 'my-animation';

	// TODO Is this the best way to model this?
	let keyframes = [{ length: 2 }, { length: 2 }, { length: 2 }];

	$: calculatedLength =
		keyframes.reduce((a, b) => a + (b['length'] || 0), 0) + timeUntilFinalKeyframe;

	$: keyframeDeclarations = keyframes.map((stage) => {
		return `\t${calculatePercentage(stage.length)}% {\n\t}\n`;
	});

	//let newStageName = '';
	let timeUntilNewKeyframe = 0;
	let timeUntilFinalKeyframe = 2;

	function addStage() {
		if (newStageName.length > 0) {
			keyframes = [...keyframes, { name: newStageName, length: timeUntilNewKeyframe }];
			newStageName = '';
		}
	}

	function removeStage(i) {
		keyframes = keyframes.filter((keyframes, index) => {
			return i !== index;
		});
	}

	function calculatePercentage(length) {
		return Math.round(((length + 1) / calculatedLength) * 100);
	}
</script>

<header>
	<h1>Keyframe Builder</h1>
</header>
<main>
	<section>
		<!-- <article>
			<label for="AnimationName">Animation Name</label>
			<input id="AnimationName" type="text" bind:value={animationName} />
		</article> -->

		<article>
			<h4>Add a new keyframe</h4>
			<!-- <input type="text" bind:value={newStageName} /> -->
			<label for="NewStageLength">Seconds until this keyframe</label>
			<input type="number" bind:value={timeUntilNewKeyframe} />
			<button on:click={addStage}>Add keyframe</button>
		</article>

		<article>
			<h4>Keyframes</h4>
			<div class="keyframe">
				<p>0%</p>
			</div>
			{#each keyframes as { length }, i}
				<fieldset class="keyframe">
					<label for="StageLength[{i}]">{calculatePercentage(length)}%</label>
					<input type="number" id="StageLength[{i}]" bind:value={length} />
					{#if i !== 0}
						<button
							on:click={() => removeStage(i)}
							aria-label="Remove animation stage"
							class="close-button">❌</button
						>
					{/if}
				</fieldset>
			{/each}
			<fieldset class="keyframe">
				<label for="FinalKeyframe">100%</label>
				<input type="number" id="FinalKeyframe" bind:value={timeUntilFinalKeyframe} />
			</fieldset>
		</article>
	</section>
	<!-- <pre>{JSON.stringify(keyframes)}</pre> -->
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
	:global(body) {
		background: linear-gradient(0deg, rgba(232, 232, 232, 1) 0%, rgba(196, 196, 196, 1) 100%);
	}

	@media (min-width: 680px) {
		main {
			display: flex;
			flex-direction: row;
		}
	}

	h1,
	h4 {
		color: hsl(0deg, 0%, 20%);
	}

	article > h4 {
		margin-top: 0;
	}

	pre {
		tab-size: 4;
	}

	article {
		margin: 0 24px 24px 0;
		background: #fff;
	}

	article,
	pre {
		padding: 16px;
		border-radius: 16px;
		box-shadow: 0 1px 1px rgba(0, 0, 0, 0.12), 0 2px 2px rgba(0, 0, 0, 0.12),
			0 4px 4px rgba(0, 0, 0, 0.12), 0 8px 8px rgba(0, 0, 0, 0.12),
			0 16px 16px rgba(0, 0, 0, 0.12);
	}

	.keyframe {
		display: flex;
		flex-direction: row;
		align-items: center;
		border: 0;
		margin-left: 0;
		padding-left: 0;
	}

	.keyframe > p,
	.keyframe > label {
		width: 40px;
	}

	.keyframe > p {
		font-weight: 600;
	}

	.keyframe > label {
		margin-right: 8px;
	}

	.keyframe > input {
		margin: 0;
	}

	.close-button {
		background: #fff;
		border: 0;
	}
</style>
