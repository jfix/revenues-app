<script>
	import { onMount } from "svelte"
	import { Container, Row, Col, Navbar, NavbarBrand } from "sveltestrap"
	import Placeholder from './Placeholder.svelte'
	import Stats from "./Stats.svelte"
	let loading = true, 
		statsResults = {},
		overallTotal = 0, 
		firstAndLastDates = {}, 
		twoDays = {}, 
		monthlyTotals = {}, 
		lastQuarter = {}

  	onMount(async () => {
		const res = await fetch(`http://localhost:3000/revenues`);
		statsResults = await res.json()
		// let { firstAndLastDates, overallTotal, twoDays, monthlyTotals, lastQuarter } = statsResults
		loading = false
	  });
	  
	  $: console.log(`Loading status: ${loading}`)
</script>

<Navbar class="navbar navbar-expand-lg navbar-dark bg-dark">
	<NavbarBrand>
		<img src="./oecd-logo.png" width="30" height="30" alt="OECD logo">
		Money money money
	</NavbarBrand>
</Navbar>

<Container class="mt-5">
	<main>
		{#if loading}
			<Placeholder/>
		{:else}
		<Stats stats={statsResults}/>
		<Row>
			<Col xs=12 md=12 lg=6>
				<!-- <Heatmaps {heatmaps}/>	 -->
			</Col>
			<Col xs=12 md=12 lg=6>
				<!-- <Meme url={url}/> -->
			</Col>
		</Row>
		{/if}
	</main>	
</Container>
