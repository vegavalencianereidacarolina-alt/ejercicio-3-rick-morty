<script>
	let personajes = [];
	let cargando = true;
	let pagina = 1;
	let totalPaginas = 0;

	async function cargarPersonajes() {
		cargando = true;
		try {
			const respuesta = await fetch(`https://rickandmortyapi.com/api/character?page=${pagina}`);
			const datos = await respuesta.json();
			personajes = datos.results;
			totalPaginas = datos.info.pages;
		} catch (error) {
			console.error('Error:', error);
		} finally {
			cargando = false;
		}
	}

	function paginaAnterior() {
		if (pagina > 1) {
			pagina -= 1;
			cargarPersonajes();
		}
	}

	function paginaSiguiente() {
		if (pagina < totalPaginas) {
			pagina += 1;
			cargarPersonajes();
		}
	}

	cargarPersonajes();
</script>

<div class="contenedor">
	<h1>Rick and Morty Svelte</h1>
	<h2>Página: {pagina}</h2>

	<div class="botones">
		<button on:click={paginaAnterior} disabled={pagina === 1}>Anterior</button>
		<button on:click={paginaSiguiente} disabled={pagina === totalPaginas}>Siguiente</button>
	</div>

	{#if cargando}
		<p class="cargando">Cargando personajes...</p>
	{:else}
		<div class="cuadricula">
			{#each personajes as p}
				<div class="tarjeta">
					<img src={p.image} alt={p.name}>
					<h3>{p.name}</h3>
					<p>{p.species}</p>
					<p>{p.status}</p>
				</div>
			{/each}
		</div>
	{/if}
</div>

<style>
	.contenedor {
		background: #fff8dc;
		min-height: 100vh;
		padding: 20px;
		text-align: center;
		font-family: serif;
	}

	h1 {
		font-size: 28px;
		margin-bottom: 5px;
	}

	h2 {
		font-size: 20px;
		margin-top: 0;
		margin-bottom: 20px;
	}

	.botones {
		display: flex;
		justify-content: space-between;
		max-width: 900px;
		margin: 0 auto 20px;
	}

	.botones button {
		padding: 6px 16px;
		cursor: pointer;
		border: 1px solid #666;
		background: #eee;
		border-radius: 3px;
	}

	.botones button:disabled {
		opacity: 0.4;
		cursor: not-allowed;
	}

	.cargando {
		font-size: 18px;
		color: #666;
	}

	.cuadricula {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		gap: 20px;
		max-width: 900px;
		margin: 0 auto;
	}

	.tarjeta {
		background: white;
		border: 1px solid #ccc;
		border-radius: 4px;
		overflow: hidden;
	}

	.tarjeta img {
		width: 100%;
		display: block;
	}

	.tarjeta h3 {
		font-size: 15px;
		margin: 8px 0 4px;
	}

	.tarjeta p {
		font-size: 13px;
		margin: 3px 0;
		color: #444;
	}
</style>
