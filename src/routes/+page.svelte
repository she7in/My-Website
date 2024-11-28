<script>
	// @ts-nocheck
	import './page.css';
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
	<meta name="description" content="Home of this App" />
</svelte:head>

{#if mountSudoku}
	<SudokuGrid {refreshSudoku} />
{/if}
