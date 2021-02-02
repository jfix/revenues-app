<script>
    import { Line } from "svelte-chartjs";
    import { Card } from "sveltestrap";
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
        console.log(`DATA: ${JSON.stringify(outData)}`)
        return outData
    }
    const prepareDataValues = (inData) => {
        let outData = []
        inData.forEach(o => {
            outData = [...outData, Object.values(o)[0]]
        }); {
        }
        return outData;
    };

    const prepareDataLabels = (inData) => {
        let outData = []
        inData.forEach(o => {
            // console.log(`DATE: ${Object.keys(o)[0]} - ${dayjs(Object.keys(o)[0], "YYYY-MM-DD")}`)
            outData = [...outData, dayjs(Object.keys(o)[0], "YYYY-MM-DD")]
        })
        console.log(`OUTDATA: ${JSON.stringify(outData)}`)
        return outData
    };

    $: data &&
        (chartData = {
            // labels: prepareDataLabels(data),
            datasets: [
                {
                    label: "Monthly sales (in EUR)",
                    data: prepareData(data),
                    borderColor: "#3498db",
                    // barPercentage: 0.5,
                    // categoryPercentage: 1.0,
                    borderWidth: 2,
                    // borderColor: "rgba(255, 134, 159, 1)",
                },
            ],
        })

    $: data && (chartOptions = () => {
        return {
        responsive: true,
        maintainAspectRatio: false,
        layout: {
            padding: 10
        },
        tooltips: {
            callbacks: {
                label: function(tooltipItem) {
                    console.log(`TTT: ${stringify(tooltipItem.yLabel)}`)
                    let label = ` ${Math.round(parseFloat(tooltipItem.yLabel)).toLocaleString()} €`
                    return label
                }
            }
        },
        scales: {
            xAxes: [
                {
                    type: "time",
                    offset: true,
                    // time: {
                    //     unit: "day",
                    //     tooltipFormat: "D MMMM YYYY",
                    //     displayFormats: {
                    //         day: "D MMM [']YY",
                    //     },
                    // },
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
    // $: chartData && console.log(`CHARTDATA: ${JSON.stringify(chartData)}`)
</script>

<div class="mt-4">
    <h3>Last 30 days</h3>
    <Card class="mt-4 border border-secondary">
        <div class="card-header">iLibrary sales over time</div>
        <div class="card-body" style="height: 329px">
            <Line data={chartData} options={chartOptions()}/>
        </div>
    </Card>
</div>
