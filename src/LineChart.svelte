<script>
    import { Line } from "svelte-chartjs";
    import { Row, Col, Card } from "sveltestrap";
    import moment from "moment";

    import { stringify } from 'flatted'

    export let data

    let chartData, chartOptions

    const prepareData = (inData) => {
        let outData = []
        inData.forEach(o => {
            outData = [...outData, {
                x: moment(Object.keys(o)[0], "YYYY-MM-DD"),
                y: Object.values(o)[0]
            }]
        })
        return outData
    }

    $: data &&
        (chartData = {
            // labels: prepareDataLabels(data),
            datasets: [
                {
                    label: "Daily sales (in EUR)",
                    data: prepareData(data),
                    borderColor: "#3498db",
                    pointRadius: 2,
                    pointHoverRadius: 4,
                    borderWidth: 2,
                    lineTension: 0,
                },
            ],
        })

    $: data && (chartOptions = () => {
        return {
        responsive: true,
        maintainAspectRatio: false,
        layout: {
            padding: 5
        },
        tooltips: {
            callbacks: {
                label: function(tooltipItem) {
                    return ` ${Math.round(parseFloat(tooltipItem.yLabel)).toLocaleString()} €`
                }
            }
        },
        scales: {
            xAxes: [
                {
                    type: "time",
                    offset: true,
                    time: {
                        unit: "day",
                        tooltipFormat: "ddd, D MMMM YYYY",
                        displayFormats: {
                            day: "D MMM",
                        },
                    },
                    gridLines: {
                        display: true,
                        color: "rgba(0, 0, 0, 0.1)",
                    },
                },
            ],
            yAxes: [
                {
                    gridLines: {
                        display: true,
                        color: "rgba(0, 0, 0, 0.1)",
                    },
                    ticks: {
                        beginAtZero: true,
                    },
                },
            ],
        },
    }
    })
</script>

<div>
    <Row>
        <Col>
            <h3 class="mt-3">Last 30 days</h3>
        </Col>
    </Row>
    <Card class="mt-4 border border-secondary">
        <div class="card-header">iLibrary sales over time</div>
        <div class="card-body" style="height: 333px">
            <Line data={chartData} options={chartOptions()}/>
        </div>
    </Card>
</div>
