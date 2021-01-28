<script>
	export let passwd = "";
	$: strength = 0;
	$: hidden = false;
	export let upperLower = false;
	export let numbers = false;
	export let special = false;
	export let eightLong = false;
	export let twelveLong = false;
	function checkPasswd() {
		strength = 0;

		if (/[A-Z]/.test(passwd) && /[a-z]/.test(passwd)) {
			strength = strength + 1;
			upperLower = true;
		} else {
			upperLower = false;
		}

		if (passwd.length >= 12) {
			strength = strength + 2;
			eightLong = true;
			twelveLong = true;
		} else if (passwd.length >= 8) {
			strength = strength + 1;
			eightLong = true;
			twelveLong = false;
		} else {
			eightLong = false;
			twelveLong = false;
		}

		if (/[0-9]/.test(passwd)) {
			strength = strength + 1;
			numbers = true;
		} else {
			numbers = false;
		}

		if (/[!@#$%^&*?]/.test(passwd)) {
			strength = strength + 1;
			special = true;
		} else {
			special = false;
		}
		console.log(strength);
	}
</script>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>

<main>
	{#if (hidden == true)}
	<input
	type="password"
	default=""
	bind:value={passwd}
	on:input={() => checkPasswd()} />
	{:else}
	<input
		type="text"
		default=""
		bind:value={passwd}
		on:input={() => checkPasswd()} />
	{/if}
	<h1>Siła twojego hasła to: {strength}/5</h1>
	{#if upperLower == false}
		<h2>Użyj małych i dużych liter</h2>
	{/if}
	{#if numbers == false}
		<h2>Użyj cyfr</h2>
	{/if}
	{#if special == false}
		<h2>
			Użyj znaków specjalnych jak:
			<bold>!@#$%^&*?</bold>
		</h2>
	{/if}
	{#if eightLong == false}
		<h2>
			Użyj minimum
			<bold>8</bold>
			znaków, lub najlepiej
			<bold>12</bold>
			znaków
		</h2>
	{:else if twelveLong == false}
		<h2>
			Użyj najlepiej
			<bold>12</bold>
			znaków
		</h2>
	{/if}
	<h2><input type=checkbox bind:checked={hidden} >ukryj hasło</h2>
</main>
