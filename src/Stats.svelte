<script>
    import Stat from "./Stat.svelte";
    import ComparativeStat from "./ComparativeStat.svelte";
    import MinMaxAvg from "./MinMaxAvg.svelte";
    import { Row, Col } from "sveltestrap";
    import dayjs from "dayjs";

    export let stats;
</script>

{#await stats then stats}
    <Col>
        <Row>
            <Col>
                <h3 class="mt-3">Numbers and trends</h3>
            </Col>
        </Row>
        <ComparativeStat header={"Latest sales"} data={stats.twoDays} />
        <MinMaxAvg header={"Nerd stats"} data={stats.minMaxAvg} />
        <!-- <Stat
            header={`All-time sales`}
            data={stats.overallTotal}
            legend={`Total sales since ${dayjs(
                stats.firstAndLastDates.firstDate
            ).format("ddd, D MMM YYYY")}`}
        /> -->
    </Col>
{/await}
