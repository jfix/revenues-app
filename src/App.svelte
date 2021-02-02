<script>
	import { onMount } from "svelte"
	import { Container, Row, Col, Navbar, NavbarBrand } from "sveltestrap"
import Chart from "./Chart.svelte"
	import Placeholder from './Placeholder.svelte'
	import Stats from "./Stats.svelte"
	import Bar from "./Chart.svelte"

	let loading = true, 
		statsResults = {},
		overallTotal = 0, 
		firstAndLastDates = {}, 
		twoDays = {}, 
		monthlyTotals = {}, 
		lastQuarter = {}

	const PRODUCTION = false

  	onMount(async () => {
		const res = await fetch(`http://localhost:3000/revenues`);
		statsResults = await res.json()
		monthlyTotals = statsResults.monthlyTotals
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
			<Col xs=12 md=12 lg=12>
				<Bar data={monthlyTotals}/>
			</Col>
		</Row>
		{/if}
	</main>	
</Container>

{#if (!PRODUCTION)}
<div style="
    position: fixed;
    top: 90%;
    left: 50%;
    z-index: 10000;
    transform: translate(-50%, -50%);
    background: rgba(247, 201, 241, 0.4);
    padding: .5rem 1rem;
    border-radius: 30px;
">
    <div class="d-block d-sm-none">Extra Small (xs)</div>
    <div class="d-none d-sm-block d-md-none">Small (sm)</div>
    <div class="d-none d-md-block d-lg-none">Medium (md)</div>
    <div class="d-none d-lg-block d-xl-none">Large (lg)</div>
    <div class="d-none d-xl-block d-xxl-none" >X-Large (xl)</div>
    <div class="d-none d-xxl-block" >XX-Large (xxl)</div>
</div>
{/if}
