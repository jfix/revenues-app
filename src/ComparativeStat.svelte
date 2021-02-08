<script>
    import { Row, Col, Card, Icon, Badge, Tooltip } from "sveltestrap";
    import dayjs from "dayjs";
    import relativeTime from "dayjs/plugin/relativeTime";

    dayjs.extend(relativeTime);

    export let data, header;
    let color,
        icon,
        latestSaleDate,
        latestSaleValue,
        previousSaleDate,
        previousSaleValue

    const trend = (today, yesterday) => {
        const diff = today - yesterday
        const threshold = 5
        const max = Math.max(today, yesterday) 
        const min = Math.min(today, yesterday) 
        const percentage = (max - min) * 100 / max
        if (!(percentage >= threshold)) {
            // not noteworthy difference
            return ["warning", "arrow-right", percentage]
        }
        if (diff < 0) {
            // worse
            return ["danger", "arrow-down-right", percentage]
        } else {
            // better
            return ["success", "arrow-up-right", percentage]
        }
    }

    $: data &&
        ((latestSaleValue = Object.values(data[1])[0]),
        (latestSaleDate = dayjs(Object.keys(data[1])[0]).format(
            "ddd, D MMM YYYY"
        )),
        (previousSaleValue = Object.values(data[0])[0]),
        (previousSaleDate = dayjs(Object.keys(data[0])[0]).format(
            "ddd, D MMM YYYY"
        )));
    $: data && ([color, icon] = trend(latestSaleValue, previousSaleValue));
</script>

<Card class="text-white bg-dark border-secondary {color} mt-4">
    <div class="card-header">
        {header}
        <Badge pill href="#" color="light" id="help1" class="ml-1">?</Badge>
        <Tooltip target="help1" trigger="hover" placement="top">
            <div>The displayed amounts have been rounded to the nearest Euro.</div>
        </Tooltip>
    </div>
    <div class="card-body">
        <Row>
            <Col xs="4" lg="4" class="align-self-end d-md-none d-lg-block">
                <h5 class="text-nowrap card-title text-center">
                    {Math.round(previousSaleValue).toLocaleString()} €
                </h5>
                <p class="text-center mb-0"><small class="text-muted">{previousSaleDate}</small></p>
            </Col>
            <Col xs="2" md="6" lg="2">
                <div class="trend-arrow text-center text-{color}">
                    <Icon name={icon} />
                </div>
            </Col>
            <Col xs="6" class="align-self-end">
                <h4 class="text-nowrap card-title text-center bignumber-header text-{color}">
                    {Math.round(latestSaleValue).toLocaleString()} €
                </h4>
                <p class="text-center mb-0"><small class="text-muted">{latestSaleDate}</small></p>
            </Col>
        </Row>
    </div>
</Card>

<style>
    .bignumber-header, .trend-arrow {
        font-size: 300%;
        font-weight: bold;
    }
    h5 {
        font-weight: bold;
        font-size: 200%;
    }
</style>
