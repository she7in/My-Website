<script>
	// @ts-nocheck
	import Sudoku from '../utils/sudoku.js';

	let K = 60; // Number of digits to remove
	let sudoku = new Sudoku(K);
	sudoku.fillValues();
	let sudokuValues = sudoku.getSudokuValues();
	let matrixValues = sudoku.getCompleteMatrix();

	let unFilledCells = $state(sudoku.getUnfilledCells());
	let mistakes = $state(0);

	let count = $state(0);
	let isCompleted = $state(false);
	let showModal = $state(false);

	let { refreshSudoku } = $props();

	const cellClass = (
		/** @type {number} */ cellIndex,
		/** @type {number} */ rowIndex,
		/** @type {any} */ cell
	) => {
		const boxBorderedCells = [2, 5, 8];
		return `h-8 w-8 flex items-center justify-center text-center ${!boxBorderedCells.includes(rowIndex) && 'border-b-2 border-b-gray-200'} ${cell ? 'bg-slate-200' : 'bg-white'} ${!boxBorderedCells.includes(cellIndex) && 'border-r-2 border-r-gray-200'}`;
	};

	const closeModal = () => {
		showModal = false;
		refreshSudoku();
	};

	const handleInput = (
		/** @type {number} */ row,
		/** @type {number} */ col,
		/** @type {number} */ value
	) => {
		let isValid = false;
		if (value) {
			if (!isNaN(value)) {
				isValid = value === matrixValues[row][col];
				unFilledCells = {
					...unFilledCells,
					[`${row}-${col}`]: { value, valid: isValid }
				};
			} else {
				unFilledCells = {
					...unFilledCells,
					[`${row}-${col}`]: { value: '', valid: false }
				};
			}
		} else {
			unFilledCells = {
				...unFilledCells,
				[`${row}-${col}`]: { value: '', valid: false }
			};
		}
		if (value && !isValid) mistakes += 1;
		const isAllFilled =
			Object.entries(unFilledCells).filter(([key, value]) => !value.valid).length === 0;
		if (isAllFilled && isCompleted !== isAllFilled) {
			isCompleted = isAllFilled;
			showModal = true;
		}
	};
</script>

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
							class={`${cellClass(cellIndex, rowIndex, cell)} ${!unFilledCells[`${rowIndex}-${cellIndex}`].valid ? 'text-red-700' : 'text-black'}`}
							bind:value={unFilledCells[`${rowIndex}-${cellIndex}`].value}
							oninput={(e) => {
								handleInput(rowIndex, cellIndex, Number(e.target.value));
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
	<div>Mistakes: {mistakes}</div>
</section>
{#if showModal}
	<div>
		<!-- Backdrop -->
		<div class="backdrop"></div>

		<!-- Modal -->
		<div class="modal">
			<div class="modal-header">congo</div>
			<div class="modal-body">oh yeah!</div>
			<div class="modal-footer">
				<button class="close-button" onclick={closeModal}>Close</button>
			</div>
		</div>
	</div>{/if}
