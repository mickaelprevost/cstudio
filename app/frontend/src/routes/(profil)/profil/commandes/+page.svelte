<script>
	// @ts-nocheck
	import '$lib/styles/app.scss';

    /** @type {import('./$types').LayoutServerData} */
	export let data;
	let orders = data.orders
</script>

<section class="order-content">
	{#if orders.length > 0}
	{#each orders as order}
		<div class="order-content__row">
			<h3>Commande n°{order.number}</h3>
			<p>du {order.deliveryDate}</p>
		</div>
	{/each}
	{:else}
		<p>Pas encore de commandes ? Devenez collectionneur d'art en un clic !</p>
		<a href="/" class="order-content__link">Visiter notre galerie</a>
	{/if}
</section>

<style lang="scss">
	.order-content {
		padding: 1rem 1.5rem;
		margin-top: 1rem;
		width: 100%;
		height: fit-content;
		display: flex;
		flex-direction: column;
		gap: 1rem;
		border-radius: 0.5rem;
		background: gainsboro;
		&__separator {
			width: 100%;
			height: 1px;
			background: darkgray;
		}
		&__link {
			width: fit-content;
			padding: 0.25rem 1.75rem;
			font-weight: 700;
			font-size: 1.25rem;
			border-radius: 0.5rem;
			background: white;
			transition: 0.3s ease-in-out;
			&:hover {
				color: gray;
			}
		}
	}
</style>
