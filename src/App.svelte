<script>
	import { onMount } from "svelte";
	import { Container, Row, Col, Navbar, NavbarBrand } from "sveltestrap";
	import Placeholder from "./Placeholder.svelte";
	import Stats from "./Stats.svelte";
	import BarChart from "./BarChart.svelte"
	import LineChart from "./LineChart.svelte"
	
	let loading = true,
		statsResults = {},
		overallTotal = 0,
		firstAndLastDates = {},
		twoDays = {},
		monthlyTotals = {},
		lastQuarter = {};

	const PRODUCTION = process.env.isProd;

	onMount(async () => {
		const res = await fetch(process.env.REVENUES_API_URL);
		statsResults = await res.json();
		monthlyTotals = statsResults.monthlyTotals
		lastQuarter = statsResults.lastQuarter
		// let { firstAndLastDates, overallTotal, twoDays, monthlyTotals, lastQuarter } = statsResults
		loading = false;
	});
</script>

<Navbar class="navbar navbar-expand-lg navbar-dark bg-dark">
	<NavbarBrand>
		<img src="./oecd-logo.png" width="30" height="30" alt="OECD logo" />
		"Money money money" <small class="pl-2 text-smaller text-muted">&mdash; you know ... from that ABBA song</small>
	</NavbarBrand>
</Navbar>

<Container class="mt-5">
	<main>
		{#if loading}
			<Placeholder />
		{:else}
			<Row>
				<Stats stats={statsResults} />
				<Col md="6">
					<LineChart data={lastQuarter.slice(-30)}/>
				</Col>
			</Row>
			<Row>
				<Col xs="12" md="12" lg="12">
					<BarChart data={monthlyTotals} />
				</Col>
			</Row>
		{/if}
	</main>
	<footer class="text-muted mt-8">
		<Container>
			<p class="text-right">This is not an official project. Ask for new features and support at your own peril! 😉</p>
		</Container>
	</footer>
</Container>

{#if !PRODUCTION}
	<div
		style="
    position: fixed;
    top: 90%;
    left: 50%;
    z-index: 10000;
    transform: translate(-50%, -50%);
    background: rgba(247, 201, 241, 0.4);
    padding: .5rem 1rem;
    border-radius: 30px;
"
	>
		<div class="d-block d-sm-none">Extra Small (xs)</div>
		<div class="d-none d-sm-block d-md-none">Small (sm)</div>
		<div class="d-none d-md-block d-lg-none">Medium (md)</div>
		<div class="d-none d-lg-block d-xl-none">Large (lg)</div>
		<div class="d-none d-xl-block d-xxl-none">X-Large (xl)</div>
		<div class="d-none d-xxl-block">XX-Large (xxl)</div>
	</div>
{/if}

<style>
	footer {
		padding: 3rem 0;
	}
</style>