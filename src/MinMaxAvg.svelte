<script>
    import { Row, Col, Card, Icon, Badge, Tooltip } from "sveltestrap";
    import dayjs from "dayjs";

    export let data, header
</script>

<Card class="text-white bg-dark border-secondary mt-4">
    <div class="card-header">
        {header}
        <Badge pill href="#" color="light" id="help3" class="ml-1">?</Badge>
        <Tooltip target="help3" trigger="hover" placement="top">
            <div>The displayed amounts have been rounded to the nearest Euro.</div>
        </Tooltip>
    </div>
    <div class="card-body">
        <Row>
            <Col xs="12" lg="4" md="12" class="align-self-end d-lg-block divider">
                <p class="text-center card-text mb-0">Max</p>
                <h5 class="text-nowrap card-title text-center">
                    {Math.round(data.max.revenue).toLocaleString()} €
                </h5>
                <p class="text-center mb-0"><small class="text-muted">{dayjs(data.max.date).format('ddd, D MMM YYYY')}</small></p>
            </Col>
            <Col xs="12" md="12" lg="4" class="d-lg-block divider">
                <p class="text-center card-text mb-0">Average</p>
                <h5 class="text-nowrap card-title text-center">
                    {Math.round(data.avg).toLocaleString()} €
                </h5>
            </Col>
            <Col xs="12" md="12" lg="4" class="align-self-end d-lg-block">
                <p class="text-center card-text mb-0">Min</p>
                <h5 class="text-nowrap card-title text-center">
                    {Math.round(data.min.revenue).toLocaleString()} €
                </h5>
                <p class="text-center mb-0"><small class="text-muted">{dayjs(data.min.date).format('ddd, D MMM YYYY')}</small></p>
            </Col>
        </Row>
    </div>
</Card>

<style>
    .bignumber-header {
        font-size: 300%;
        font-weight: bold;
    }
    .trend-arrow {
        font-size: 300%;
        font-weight: bold;
    }
    h5 {
        font-weight: bold;
        font-size: 200%;
    }
    .divider {
        border-right: 1px dashed white;
    }
</style>
