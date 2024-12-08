<script>
	// @ts-nocheck

	let F = $state(0)
	let C = $state(0)
	function f2c() {
		C = ((F - 32) * 5) / 9
	}
</script>

<input type="number" bind:value={F} onchange={f2c} />
<input type="number" bind:value={C} />

{F}, {C}
