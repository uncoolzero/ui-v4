<script>
	/*
	TODO:
		- similar display and UI as the Trade page
		- Should display the pool balance, buffer balance, user's pool balance on the left side
		- Should have an add liquidity and remove liquidity form on the right side
	*/

	import { onDestroy } from 'svelte';
	import Pools from './Pools.svelte'
	import Transactions from './Transactions.svelte'
	import { address } from '@lib/stores'
	import { getPoolBalance, getBufferBalance, getPoolWithdrawalFee, getUserPoolBalance } from '@api/pool'


	let isLoading = true, t;

	async function fetchData() {
		clearTimeout(t);
		const done1 = await getPoolBalance();
		const done2 = await getBufferBalance();
		const done3 = await getPoolWithdrawalFee();
		const done4 = await getUserPoolBalance();
		t = setTimeout(fetchData, 5*1000);
	}

	$: fetchData($address)

	onDestroy(() => {
		clearTimeout(t);
	});

$: fetchData($address)

</script>

<style>
	.grid {
		display: grid;
		grid-template-rows: auto auto;
		grid-template-columns: repeat(4, 1fr);
		grid-template-areas: 
			"header header header header"
			"pools transactions transactions transactions";
		grid-gap: 0px;
		background-color: var(--layerDark);
		max-width: var(--container-width);
		margin: 0 auto;
	}

	.pools {
		grid-area: pools;
		background-color: var(--layer50);
		border: 1px solid var(--layerDark);
		border-top: 0px;
		border-right: 0px;
	}
	.transactions {
		grid-area: transactions;
		background-color: var(--layer50);
		border: 1px solid var(--layerDark);
		border-top: 0px;
	}
	.header {
		grid-area: header;
		background-color: var(--layer50);
		border: 1px solid var(--layerDark);
		height: 60px;
		padding: 0 20px;
		display: flex;
		flex-direction: row;
		gap: 15px;
	}
	.title {
		font-weight: 600;
		font-size: 16px;
		align-self: center;
		text-transform: capitalize;
	}
	.subtitle {
		color: var(--text300);
		font-size: 14px;
		align-self: center;
		margin-top: 1px;
	}

	@media (max-width: 650px) {
		.grid {
			grid-template-rows: auto;
			grid-template-columns: 1fr;
			grid-template-areas: 
				"header"
				"pools"
				"transactions";
			width: 100vw;
			margin: 0 auto;
			overflow-x: hidden;
		}
		.pools {
			border: 1px solid var(--layerDark);
			border-top: 0px;
		}
		.transactions {
			border: 1px solid var(--layerDark);
			border-top: 0px;
		}
		.header {
			padding: 0 10px;
		}
	}


</style>

<div class='grid'>
	<div class='header'>
		<div class='title'>Pool</div>
		<div class='subtitle'>The Pool pays out trader wins and receives losses plus fees.</div>
	</div>
	<div class='pools'><Pools /></div>
	<div class='transactions'><Transactions /></div>
</div>