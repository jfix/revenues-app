<script>
    import { Row, Col, Card, Icon, Badge, Popover } from "sveltestrap";
    import dayjs from "dayjs";
    import relativeTime from "dayjs/plugin/relativeTime";

    dayjs.extend(relativeTime);

    export let data, header;
    let color,
        icon,
        latestSaleDate,
        latestSaleValue,
        previousSaleDate,
        previousSaleValue;

    const trend = (today, yesterday) => {
        const diff = today - yesterday;
        console.log(`diff of ${today} - ${yesterday} = ${diff}`);
        if (diff < 0) {
            // worse
            return ["danger", "arrow-down-right"];
        } else if (diff > 0) {
            // better
            return ["success", "arrow-up-right"];
        } else {
            // same
            return ["warning", "arrow-right"];
        }
    };

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

<Card class="text-white bg-{color} mt-4">
    <div class="card-header">
        {header}
        <Badge pill color="light" id="help1" class="ml-1">?</Badge>
        <Popover target="help1" trigger="hover" placement="top">
            <div>Hello World!</div>
        </Popover>
    </div>
    <div class="card-body">
        <Row>
            <Col xs="6" class="align-self-end">
                <h4 class="text-nowrap card-title text-center bignumber-header">
                    {Math.round(latestSaleValue)} €
                </h4>
                <p class="text-center card-text">{latestSaleDate}</p>
            </Col>
            <Col xs="2" md="6" lg="2">
                <div class="trend-arrow text-center">
                    <Icon name={icon} />
                </div>
            </Col>
            <Col xs="4" lg="4" class="align-self-end d-md-none d-lg-block">
                <h5 class="text-nowrap card-title text-center">
                    {Math.round(previousSaleValue)} €
                </h5>
                <p class="text-center card-text">{previousSaleDate}</p>
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
</style>
