<script>
    import Stat from "./Stat.svelte"
    import ComparativeStat from "./ComparativeStat.svelte"
    import { Row } from "sveltestrap"
    import dayjs from 'dayjs'

    export let stats

    $: stats && console.log(`First date: ${JSON.stringify(stats.firstAndLastDates)}`)
</script>

<h3 class='mt-3'>Some sales numbers</h3>
<Row>
    {#await stats then stats}
        <Stat header={`iLibrary e-commerce`} 
            data={`${parseFloat(stats.overallTotal).toLocaleString()} €`}
            legend={`Total sales since ${dayjs(stats.firstAndLastDates.firstDate).format('ddd, D MMM YYYY')}`}
        />
        <ComparativeStat header={'Latest sales'}
            data={stats.twoDays}
        />
    {/await}
</Row>
