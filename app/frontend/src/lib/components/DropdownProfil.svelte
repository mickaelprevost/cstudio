<script>
	// @ts-nocheck

	// Import generals style
	import '$lib/styles/app.scss';

	// Import module
	import { page } from '$app/stores';

	// Declare variables
	const user = $page.data.session.user;
</script>

<aside class="dropdown-profil">
	{#if user !== undefined}
		<h4 class="dropdown-profil__title">{user.firstName} {user.lastName}</h4>
		<div class="dropdown-profil__separator" />
		<a href="/profil" class="dropdown-profil__link">Profil</a>
		{#if user.role[0] === 'ROLE_ADMIN'}
			<a href="/admin" class="dropdown-profil__link">Tableau de bord</a>
		{/if}
		<div class="dropdown-profil__separator" />
		<form action="/deconnexion?/logout" method="post">
			<button class="dropdown-profil__link">Déconnexion</button>
		</form>
	{:else}
		<a href="/connexion" class="dropdown-profil__login">connexion</a>
		<a href="/inscription" class="dropdown-profil__signup">pas encore de compte ?</a>
	{/if}
</aside>

<style lang="scss">
	@use '../styles/variables' as *;
	.dropdown-profil {
		z-index: 1000;
		position: absolute;
		top: 7rem;
		right: 1rem;
		padding: 0.75rem 1rem;
		width: fit-content;
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		gap: 0.5rem;
		border-radius: 0.5rem;
		border: 1px solid rgba(255, 255, 255, 0.15);
		backdrop-filter: blur(3.1px);
		-webkit-backdrop-filter: blur(3.1px);
		box-shadow: 0 4px 30px rgba(0, 0, 0, 0.15);
		background: rgba(255, 255, 255, 0.9);
		&__separator {
			width: 100%;
			height: 1px;
			background: black;
		}
		&__link {
			background: none;
			&:hover {
				color: lightslategray;
			}
		}
		&__login {
			padding-block: 0.25rem;
			width: 100%;
			display: flex;
			justify-content: center;
			align-items: center;
			font-weight: 700;
			font-size: 1.1rem;
			border-radius: 0.5rem;
			background: $color-green;
			transition: 0.15s ease-in-out;
			&:hover {
				color: $color-white;
			}
		}
		&__signup {
			width: 100%;
			font-size: 0.9rem;
			text-align: center;
			transition: 0.15s ease-in-out;
			&:hover {
				color: $color-green;
			}
		}
	}
</style>
