<script>
	// @ts-nocheck
	import './page.css';

	import welcome from '$lib/images/svelte-welcome.webp';
	import welcomeFallback from '$lib/images/svelte-welcome.png';
	import SudokuGrid from '../components/SudokuGrid.svelte';

	let mountSudoku = $state(true);

	const refreshSudoku = async () => {
		mountSudoku = false; // Set mountSudoku to false
		setTimeout(() => {
			mountSudoku = true; // Reset mountSudoku to true after 1 second
		}, 0);
	};
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<h1>
		<span class="welcome">
			<picture>
				<source srcset={welcome} type="image/webp" />
				<img src={welcomeFallback} alt="Welcome" />
			</picture>
		</span>
		Shehin Dileep
	</h1>
</section>

{#if mountSudoku}
	<SudokuGrid {refreshSudoku} />
{/if}
