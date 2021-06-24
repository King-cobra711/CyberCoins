<script>
	import { onMount } from "svelte";
    import { each } from "svelte/internal";
    

    // export let coinData;
	
	const coinUrl = "https://api.coingecko.com/api/v3/coins/markets?vs_currency=AUD&order=market_cap_desc&per_page=10&page=1&sparkline=false&price_change_percentage=1h%2C24h%2C7d";

	let coins = [];
	onMount(async () => {
    const res = await fetch(coinUrl)
	coins = await res.json();
	})
	const refresh = (async () => {
	coins = [];
    const res = await fetch(coinUrl)
	coins = await res.json();
	});
</script>

<main class="text-center">
	<h1 class="bg-blue-900 pb-3 pt-8 text-5xl sm:text-7xl lg:text-9xl text-white ">CyberCoins</h1>
	<div class="flex justify-center bg-blue-900 py-2">
		<svg on:click={refresh} xmlns="http://www.w3.org/2000/svg" class="h-10 w-10 text-white bg-blue-900" viewBox="0 0 20 20" fill="currentColor">
  <path fill-rule="evenodd" d="M4 2a1 1 0 011 1v2.101a7.002 7.002 0 0111.601 2.566 1 1 0 11-1.885.666A5.002 5.002 0 005.999 7H9a1 1 0 010 2H4a1 1 0 01-1-1V3a1 1 0 011-1zm.008 9.057a1 1 0 011.276.61A5.002 5.002 0 0014.001 13H11a1 1 0 110-2h5a1 1 0 011 1v5a1 1 0 11-2 0v-2.101a7.002 7.002 0 01-11.601-2.566 1 1 0 01.61-1.276z" clip-rule="evenodd" />
</svg>
	</div>
	
	<div class="flex bg-blue-900 justify-center">
	{#if coins.length === 0}
	<div class="bg-blue-900 justify-center m-auto	">
	<img src="/spinner.gif" alt="loading"  class="w-32 h-32 my-48"/>
	</div>
	{:else}
	<div class="flex flex-wrap bg-blue-900 justify-center m-auto">
	{#each coins as coin }
		<div class=" bg-gray-200 rounded shadow-md m-2 p-2 max-w-md w-full sm:w-80 ">
			<img class="w-32 h-32 rounded-full mx-auto" src={coin.image} alt="crypto coin" width="384" height="512">
			<div class="grid grid-rows-2 grid-flow-col gap-1">
			<div class="row-span-1 col-span-1 ..."><h2 class="capitalize font-bold m-0.5 flex-auto">{coin.id}</h2></div>
            <div class="row-span-1 ..."><p class="flex-auto {coin.price_change_percentage_1h_in_currency < 0 ? 'text-red-600' : 'text-green-600'} m-0.5">{Math.round(coin.price_change_percentage_1h_in_currency * 100) / 100}%</p></div>
            {#if coin.price_change_percentage_1h_in_currency < 0}
                <svg xmlns="http://www.w3.org/2000/svg" class="text-red-600 justify-self-end row-span-2 h-12 w-12 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13               17h8m0 0V9m0 8l-8-8-4 4-6-6" /></svg>
				{:else}
				{#if coin.price_change_percentage_1h_in_currency = 0}
				<p>-</p>
				{:else}
				<svg xmlns="http://www.w3.org/2000/svg" class="text-green-600 justify-self-end row-span-2 h-12 w-12 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round"               stroke-linejoin="round" stroke-width="2" d="M13               7h8m0 0v8m0-8l-8 8-4-4-6 6" /></svg>
				{/if}
				{/if}
			
		</div>
			<!-- <div class="flex"><h2 class="capitalize font-bold m-0.5 flex-auto">{coin.id}</h2> 
			<p class="flex-auto text-right pr-4">timer</p>
			</div>
			<div class="flex">
				<p class="flex-auto {coin.price_change_percentage_1h_in_currency < 0 ? 'text-red-600' : 'text-green-600'} m-0.5">{Math.round(coin.price_change_percentage_1h_in_currency * 100) / 100}%</p>
				<p class="flex-auto text-right pr-4">{Date.now() - new Date(coin.last_updated)}</p>
			</div> -->
			
			<p>Current price: ${Math.round(coin.current_price * 1000) / 1000} (AUD)</p>
		</div>
	{/each}
</div>;
	{/if}
</div>
</main>

<style global lang="postcss">
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
</style>