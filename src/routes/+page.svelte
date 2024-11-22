<script>
	// @ts-nocheck

	import welcome from '$lib/images/svelte-welcome.webp';
	import welcomeFallback from '$lib/images/svelte-welcome.png';
	import Sudoku from '../utils/sudoku.js';

	let K = 40; // Number of digits to remove
	let sudoku = new Sudoku(K);
	sudoku.fillValues();
	let sudokuValues = sudoku.getSudokuValues(); // Return the sudoku grid
	let matrixValues = sudoku.getCompleteMatrix(); // Return the completes matrix grid

	sudoku.printSudoku();
	console.log('\nnext\n');
	sudoku.printComplete();

	let unFilledCells = $state(sudoku.getUnfilledCells());

	let count = $state(0);

	const cellClass = (
		/** @type {number} */ cellIndex,
		/** @type {number} */ rowIndex,
		/** @type {any} */ cell
	) => {
		const boxBorderedCells = [2, 5, 8];
		return `h-8 w-8 flex items-center justify-center text-center ${!boxBorderedCells.includes(rowIndex) && 'border-b-2 border-b-gray-200'} ${cell ? 'bg-slate-200' : 'bg-white'} ${!boxBorderedCells.includes(cellIndex) && 'border-r-2 border-r-gray-200'}`;
	};

	const handleInput = (
		/** @type {number} */ row,
		/** @type {number} */ col,
		/** @type {number} */ val
	) => {
		const value = Number(val);
		console.log('sssss', value === matrixValues[row][col], value, matrixValues[row][col]);
		if (value && isNaN(value))
			unFilledCells = {
				...unFilledCells,
				[`${row}-${col}`]: { value, valid: value === matrixValues[row][col] }
			};
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
<section>
	<div>
		{#each sudokuValues as row, rowIndex}
			<div class="flex justify-center align-middle">
				{#each row as cell, cellIndex}
					{#if cell}
						<span class={cellClass(cellIndex, rowIndex, cell)}>
							{cell}
						</span>
					{:else}
						<input
							type="text"
							value={unFilledCells[`${rowIndex}-${cellIndex}`].value || ''}
							class={`${cellClass(cellIndex, rowIndex, cell)} ${!unFilledCells[`${rowIndex}-${cellIndex}`].valid ? 'text-red-700' : 'text-black'}`}
							onchange={(e) => {
								handleInput(rowIndex, cellIndex, e.target.value);
							}}
							maxlength="1"
						/>
					{/if}
					{#if cellIndex === 2 || cellIndex === 5}
						<div class="border-r-2 border-r-black"></div>
					{/if}
				{/each}
			</div>
			{#if rowIndex === 2 || rowIndex === 5}
				<div class="border-b-2 border-b-black"></div>
			{/if}
		{/each}
	</div>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
	}

	.welcome {
		display: block;
		position: relative;
		width: 100%;
		height: 0;
		padding: 0 0 calc(100% * 495 / 2048) 0;
	}

	.welcome img {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		display: block;
	}
</style>
