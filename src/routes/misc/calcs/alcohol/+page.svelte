<script>
	import { readonly } from 'svelte/store'
	import Button from './Button.svelte'
	import Explanation from './Explanation.svelte'
	import SGs from './SGs.svelte'
	let viewCalc = true
	let selectedOption = 'volume'
	let weight = ''
	let sg = '1.00'
	let volume = ''
	let abv = ''
	let result

	const options = [
		{ value: 'volume', label: 'Volume' },
		{ value: 'weight', label: 'Weight' }
	]

	function weightResult() {
		let result = (((Number(weight) / Number(sg)) * Number(abv)) / 1000).toFixed(2)
		return result
	}
	function volumeResult() {
		let result = ((Number(volume) * Number(abv)) / 1000).toFixed(2)
		return result
	}

	function toggleView() {
		viewCalc = !viewCalc
	}
	function calcUnits() {
		if (selectedOption === 'weight') {
			result = ((Number(weight) / Number(sg)) * Number(abv)) / 1000
		} else {
			result = (Number(volume) * Number(abv)) / 1000
		}
	}
</script>

<div class="outer">
	<div class="wrapper">
		<h1>Alcohol Units Calculator</h1>
		<button onclick={toggleView}>
			{#if viewCalc}<span class="bigly">&larr;</span>
				Show Explanation{:else}Go to Calculator <span class="bigly">&rarr;</span>
			{/if}
		</button>
		<div class:hide={viewCalc} class:show={!viewCalc}><Explanation /></div>
		<div class:show={viewCalc} class:hide={!viewCalc}>
			<div class="card drawn1">
				<div class="radioGroup">
					<h4>Are you starting with a volume or a weight?</h4>
					{#each options as option}
						<label class="radio">
							<input
								type="radio"
								name="radioGroup"
								bind:group={selectedOption}
								value={option.value}
								checked={selectedOption === option.value} />
							{option.label}
						</label>
					{/each}
				</div>
				<section class="form">
					{#if selectedOption === 'weight'}
						<div class="label">
							Weight, <strong>W</strong>
						</div>
						<div class="input">
							<input type="number" id="weight" bind:value={weight} />
							<span>g</span>
						</div>
						<div class="label">
							Specific Gravity, <strong>SG</strong>
						</div>
						<div class="input">
							<input type="number" id="sg" bind:value={sg} />
						</div>
					{:else}
						<div class="label">
							Volume, <strong>V</strong>
						</div>
						<div class="input">
							<input type="number" id="volume" bind:value={volume} />
							<span>ml</span>
						</div>
					{/if}

					<div class="label">
						Alcohol by volume, <strong>ABV</strong>
					</div>

					<div class="input">
						<input type="number" id="abv" bind:value={abv} />
						<span>%</span>
					</div>
				</section>

				{#if selectedOption === 'weight'}
					<div class="formulabox">
						Weight (g) &divide; SG &times; ABV (%) &divide; 1000 = alcohol units
					</div>
					<div class="resultbox">
						{#if weight && sg && abv}
							{weight} &divide; {sg} &times; {abv} &divide; 1000 &equals;
							<span class="result">{weightResult()}</span>
							alcohol units

							<!-- {((Number(weight) * Number(abv)) / 1000).toPrecision(1)} -->
						{/if}
					</div>
				{:else}
					<div class="formulabox">Volume (ml) &times; ABV (%) &divide; 1000 = alcohol units</div>
					<div class="resultbox">
						{#if volume && abv}
							{volume} &times; {abv} &divide; 1000 &equals;
							<span class="result">{volumeResult()}</span>
							alcohol units

							<!-- {((Number(weight) * Number(abv)) / 1000).toPrecision(1)} -->
						{/if}
					</div>
				{/if}

				<!-- <p>
					<button onclick={calcUnits}>Calculate Alcohol Units</button>
				</p> -->
			</div>
			{#if selectedOption === 'weight'}
				<h3>Click below for a list of specific gravities</h3>
				<div class="poison"><SGs /></div>
			{/if}
		</div>
	</div>
</div>

<style>
	.result {
		color: white;
		background: #a00;
		border-radius: var(--radius-2);
		font-size: 1.5rem;
		padding-inline: 1rem;
		padding-block: 0.5rem;
	}
	.resultbox {
		/* border: 1px solid black; */
		/* color: #666; */
		height: 2rem;
		margin-block: 1rem;
		font-family: 'Alkes', sans-serif;
		font-size: 1.25rem;
		font-style: italic;
		font-weight: bold;
	}
	.formulabox {
		/* border: 1px solid black; */
		margin-block: 2rem;
		font-family: 'Alkes', sans-serif;
		/* font-size: 1.25rem; */
		font-style: italic;
		/* font-weight: bold; */
	}
	.show {
		display: block;
	}
	.hide {
		display: none;
	}
	section.form {
		display: grid;
		grid-template-columns: 1fr 1fr;
		row-gap: 0.5rem;
		column-gap: 1rem;
	}
	.form .input {
		justify-self: start;
		align-self: center;
	}
	.form .label {
		justify-self: end;
		align-self: center;
	}
	.outer {
		background-color: var(--sand-2);
		margin: 0;
		padding: 1rem;
		min-height: 100vh;
	}
	.wrapper {
		display: flex;
		justify-content: start;
		align-items: center;
		flex-direction: column;
		margin-inline: auto;
		text-align: center;
		max-width: 1024px;
		padding: var(--size-3);
		max-width: 50rem;
		font-size: var(--font-size-fluid-0);
	}

	h1 {
		background-color: white;
		border: var(--border-size-3) solid #a00;
		border-radius: var(--radius-drawn-1);
		color: #a00;
		font-family: 'Carolena Narashy', sans-serif;
		font-family: 'Unseeness', sans-serif;
		font-family: 'Katherine Script', sans-serif;
		letter-spacing: 0.25rem;
		font-size: var(--font-size-6);
		padding-inline: 2rem;
		margin: 0;
	}
	h3 {
		font-size: 1rem;
		margin-block-start: 2rem;
		margin-block-end: 0.5rem;
		font-family: 'Alkes', sans-serif;
		font-style: italic;
	}
	.card {
		background: white;
		border: var(--border-size-3) solid #a00;
		box-shadow: var(--shadow-6);
		font-size: 1.5rem;
		margin: 1rem;
		padding: 1rem;
		font-family: 'Alkes', sans-serif;
		font-size: var(--font-size-2);
		font-style: italic;
		min-width: 40rem;
	}
	button {
		background: #a00;
		border: var(--border-size-3) solid #a00;
		border-radius: var(--radius-drawn-1);
		color: white;
		font-size: var(--font-size-2);
		font-weight: bold;
		letter-spacing: 0.035rem;
		margin: 1rem;
		padding: 0.5rem 1rem;
	}
	button:hover {
		background: #800;
		/* background-color: black; */
		border: var(--border-size-3) solid #800;
		/* border: var(--border-size-3) solid black; */
		/* box-shadow: var(--shadow-6); */
	}
	.bigly {
		font-size: 1.5rem;
		font-weight: bold;
		/* font-family: Arial, sans; */
	}
	.drawn1 {
		border-radius: var(--radius-drawn-1);
	}
	.drawn2 {
		border-radius: var(--radius-drawn-2);
	}
	.drawn3 {
		border-radius: var(--radius-drawn-3);
	}

	input[type='radio'] {
		appearance: none;
		width: 20px;
		height: 20px;
		border: 2px solid #333;
		border-radius: 50%;
		outline: none;
		/* align button with value text */
		vertical-align: -20%;
	}

	input[type='radio']:before {
		content: '';
		display: block;
		width: 60%;
		height: 60%;
		margin: 20% auto;
		border-radius: 50%;
	}

	input[type='radio']:checked:before {
		background: #900;
	}
	label.radio {
		display: inline-block;
		/* margin-block-start: -2rem; */
		margin-block-end: 2rem;
		margin-inline: 2rem;
	}

	input[type='number']::-webkit-outer-spin-button,
	input[type='number']::-webkit-inner-spin-button {
		-webkit-appearance: none;
		display: none;
	}

	input[type='number'] {
		-moz-appearance: textfield;
	}
</style>
