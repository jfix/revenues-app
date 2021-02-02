<script>
    import { Bar } from "svelte-chartjs";
    import { Card } from "sveltestrap";
    import moment from "moment";
    import {parse, stringify} from 'flatted'

    export let data
    let chartData, chartOptions

    const prepareDataValues = (inData) => {
        let outData = []
        for (const value of Object.values(inData)) {
            outData = [...outData, value]
        }
        return outData;
    };

    const prepareDataLabels = (inData) => {
        let outData = []
        for (const date of Object.keys(inData)) {
            outData = [...outData, moment(date, "YYYY-MM")]
        }
        return outData
    };

    $: data &&
        (chartData = {
            labels: prepareDataLabels(data),
            datasets: [
                {
                    label: "Monthly sales (in EUR)",
                    data: prepareDataValues(data),
                    backgroundColor: "#3498db",
                    // barPercentage: 0.5,
                    // categoryPercentage: 1.0,
                    // borderWidth: 2,
                    // borderColor: "rgba(255, 134, 159, 1)",
                },
            ],
        })

    chartOptions = {
        responsive: true,
        maintainAspectRatio: false,
        layout: {
            padding: 10
        },
        tooltips: {
            callbacks: {
                label: function(tooltipItem, data) {
                    console.log(`TTT: ${stringify(tooltipItem.yLabel)}`)
                    let label = ` ${Math.round(parseFloat(tooltipItem.yLabel)).toLocaleString()} €`
                    // if (label) la
                    return label
                }
            }
        },
        scales: {
            xAxes: [
                {
                    type: "time",
                    offset: true,
                    time: {
                        unit: "month",
                        tooltipFormat: "MMMM YYYY",
                        displayFormats: {
                            month: "MMM [']YY",
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
                        // beginAtZero: true,
                    },
                },
            ],
        },
    }

    $: chartData && console.log(`CHARTDATA: ${JSON.stringify(chartData)}`)
</script>

<Card>
    <div class="card-header">iLibrary sales over time</div>
    <div class="card-body" style="height: 531px">
        <Bar data={chartData} options={chartOptions} />
    </div>

</Card>
