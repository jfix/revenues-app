<script>
    import Stat from "./Stat.svelte";
    import ComparativeStat from "./ComparativeStat.svelte";
    import { Row, Col } from "sveltestrap";
    import dayjs from "dayjs";

    export let stats;

    $: stats &&
        console.log(`First date: ${JSON.stringify(stats.firstAndLastDates)}`);
</script>

{#await stats then stats}
    <Col>
        <Row>
            <Col>
                <h3 class="mt-3">Some sales numbers</h3>
            </Col>
        </Row>
        <Stat
            header={`iLibrary e-commerce`}
            data={stats.overallTotal}
            legend={`Total sales since ${dayjs(
                stats.firstAndLastDates.firstDate
            ).format("ddd, D MMM YYYY")}`}
        />
        <ComparativeStat header={"Latest sales"} data={stats.twoDays} />
    </Col>
{/await}
