<script lang="ts">
	import { writable } from 'svelte/store';
	import { scale } from 'svelte/transition';

	let resultStore = writable<string[]>();
	let matchStore = writable(false);
	let accountOne: string = '';
	let accountTwo: string = '';
	let sanitizedOne: string = '';
	$: sanitizedOne = accountOne.replace(/-/g, '');
	$: sanitizedTwo = accountTwo.replace(/-/g, '');

	let lengths = [2, 4, 7, 3];

	$: (() => {
		let result = [];
		let index = 0;
		for (let i = 0; i < lengths.length; i++) {
			let length = lengths[i];
			result.push(sanitizedOne.substr(index, length));
			index += length;
		}
		$resultStore = result;
	})();

	$: (() => {
		if (sanitizedOne === sanitizedTwo && (sanitizedOne !== '' || sanitizedTwo !== '')) {
			$matchStore = true;
			return;
		} else {
			$matchStore = false;
			return;
		}
	})();
</script>

<main class="w-screen h-screen">
	<section class="flex flex-col items-center justify-center min-h-[50%]">
		<div class="pb-10">A barebone bank account matcher</div>
		<div class="flex flex-col gap-3">
			<input
				type="text"
				placeholder="Type here"
				class="input input-bordered w-full max-w-xs"
				bind:value={accountOne}
			/>
			<input
				type="text"
				placeholder="Type here"
				class="input input-bordered w-full max-w-xs"
				bind:value={accountTwo}
			/>
		</div>
		<div class="min-h-[20px] mt-10">
			{#if $matchStore}
				<div class="bg-green-400 rounded-xl" in:scale={{ duration: 800 }}>✔</div>
			{/if}
		</div>
	</section>
</main>
