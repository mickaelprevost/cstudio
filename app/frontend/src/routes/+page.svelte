<script>
	// @ts-nocheck

	// Import generals style
	import '$lib/styles/app.scss';

	// Import modules
	import { findProductsByCategoryId, findProductsByType } from '$lib/js/request.js'; 

	// Import components
	import Header from '$lib/components/Header.svelte';
	import Card from '$lib/components/Card.svelte';
	import Footer from '$lib/components/Footer.svelte';
	import iconSearch from '$lib/icons/search.svg';

	// Import for hero
	import emblaCarouselSvelte from 'embla-carousel-svelte';
	import Autoplay from 'embla-carousel-autoplay';

	const autoplayOptions = {
		delay: 5000,
		stopOnInteraction: false,
		stopOnMouseEnter: true,
		rootNode: (emblaRoot) => emblaRoot.parentElement
	};

	let options = { dragFree: true, containScroll: 'trimSnaps', loop: true };
	let plugins = [Autoplay(autoplayOptions)];

	/** @type {import('./$types').LayoutServerData} */
	/** @type {import('./$types').PageData} */
	export let data;

	// Declare variables
		// search bar 
	let screenSize = 0;
	let isSearchOpen = false
		// data products
	let products = data.products.slice(0, 10);
	let types = data.types;
	let categories = data.categories;
	let likes = data.likes;
	console.log(data.session)

	// Declare functions
		// search bar 
		const toggleOpenSearch = () => isSearchOpen = !isSearchOpen;
		// data products 
	const onFiltredByType = async (e) => (products = await findProductsByType(e.target.dataset.typeId));
	const onFiltredByCategory = async (e) => {products = await findProductsByCategoryId(e.target.dataset.categoryId)};
	const onResetProducts = () => (products = data.products.slice(0, 10));
	
	//search
	let searchTerm = "";
	$: filteredList = products.filter(product => product.title.indexOf(searchTerm) !== -1);
	
</script>

<svelte:head>
	<title>Accueil | C-Studio - Plateform de vente en ligne d'oeuvre d'art</title>
</svelte:head>

 

<div class="wrapper">
	<Header />
	<section class="hero"> 
		<div class="embla">
			<div class="embla__viewport" use:emblaCarouselSvelte={{ options, plugins }}>
				<div class="embla__container">
					{#each types as type, i (type.id)}
					<div class="embla__slide">
						<div class="embla__img-container">
							<img class="embla__img" src="/img/slider/{type.name}.webp" alt="{type.name}"/>
							<div class="embla__content">
								<button class="embla__btn" data-type-id="{type.id}" on:click={onFiltredByType}>{type.name}</button>
							</div> 
						</div>
					</div>
					{/each}
				</div>
			</div>
		</div>
	</section>

	<main class="main">
		<div class="container">
			<h1>Découvrez toutes les œuvres de C-Studio !</h1>
			{#if isSearchOpen}
			<aside class="aside-search">
				<form class="search">
					<i class="iconoir-search search__icon" />
					<input bind:value={searchTerm} placeholder="Effectuez une recherche..." class="search__input" />
				</form>
			</aside>
			{/if}
			<div class="filter">
				<h2 class="filter__title">Sélectionnez votre catégorie :</h2>
				<nav class="filter__links">
					<button class="filter__link" on:click={onResetProducts}>Tous,</button>
					{#each categories as category, i (category.id)}
						<button class="filter__link" data-category-id="{category.id}" on:click={onFiltredByCategory}>
							{category.name.charAt(0).toUpperCase() + category.name.slice(1)}{(categories.length - 1) === i ? " " : "," }
						</button>
						{/each}
						<br>
						<button on:click={toggleOpenSearch}>
							<img src={iconSearch} alt="Icon de la recherche" class="header__icon" />
						</button>
				</nav>
			</div>
			

			<section class="list-products">
				{#each filteredList as product, i}
					{#if i === 6 || i === 7}
						<Card {...product} {likes} isBig={true} />
					{:else}
						<Card {...product} {likes} />
					{/if}
				{/each}
			</section>
		</div>
	</main>
</div>

<Footer />

<style lang="scss">
	@use '../lib/styles/variables' as *;

	.aside-search {
		width: 20vw;
		height: 3rem;
		background: $color-background;
		border-bottom: 1px solid $color-black;
	}

	.search {
		padding-block: 2rem;
		padding-left: 0.75rem;
		padding-top: 1.5rem;
		width: 100%;
		height: 3rem;
		display: flex;
		align-items: center;
		gap: 0.75rem;
		&__icon {
			font-size: 1.2rem;
		}
		&__input {
			padding-right: 0.5rem;
			width: 100%;
			font-size: 1.2rem;
			&:focus {
				outline: none;
			}
		}
	}
	.main {
		transform: translateY(-3rem);
		margin-top: 1rem;
	}
	.filter {
		margin-block: 2rem;
		display: flex;
		flex-wrap: wrap;
		gap: 1rem;
		align-items: center;
		&__title {
			font-size: 1rem;
		}
		&__links {
			display: flex;
			flex-wrap: wrap;
			gap: 0.35rem;
			font-style: italic;
		}
		&__link {
			transition: 0.1s ease-in-out;
			&:hover {
				color: $color-green;
				text-decoration: underline;
			}
		}
	}

	.list-products {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		font-size: small;
	}

	.hero {
		padding-top: 4rem;
		width: 100%;
		background: $color-yellow;
		transform: translateY(-5.25rem);
		border-bottom: 3px solid black;
	}

	.embla {
		--slide-spacing: 3rem;
		--slide-size: 70%;
		--slide-height: 12rem;
		padding-block: 1.6rem;
		&__viewport {
			z-index: 10;
			overflow: hidden;
		}
		&__container {
			backface-visibility: hidden;
			display: flex;
			touch-action: pan-y;
			margin-left: calc(var(--slide-spacing) * -1);
		}
		&__slide {
			flex: 0 0 var(--slide-size);
			min-width: 0;
			padding-left: var(--slide-spacing);
			position: relative;
			cursor: grab;
			&:active {
				cursor: grabbing;
			}
		}
		&__img {
			display: block;
			height: var(--slide-height);
			width: 100%;
			object-fit: cover;
			filter:grayscale(100%);
			border-radius: 1rem;
			&:hover {
				filter:grayscale(0);
			}
		}
		&__img-container {
			position: relative;
		}

		&__btn {
			position: absolute;
			top: 0;
			bottom:0;
			left: 0;
			right: 0;
			margin:auto;
			width: fit-content;
			height: fit-content;
			opacity: 0;
			padding: 0.25rem 1.75rem;
			font-weight: 700;
			font-size: 1rem;
			border-radius: 0.5rem;
			background: $color-white;
			transition: 0.3s ease-in-out;
			&:hover {
				color: gray;
			}
		}
		.embla__slide:hover .embla__btn {
			opacity: 1;
		}
	}

	@media screen and (min-width: 640px) {
		.list-products {
			grid-template-columns: repeat(4, 1fr);
		}
	}
	@media screen and (min-width: 900px) {
		.list-products {
			font-size: large;
		}
	}
	@media screen and (min-width: 994px) {
		.hero {
			padding-top: 5rem;
			transform: translateY(-5.2rem);
		}
		.embla {
			--slide-spacing: 6rem;
			--slide-size: 60%;
			--slide-height: 28rem;
		}
	}
</style>
