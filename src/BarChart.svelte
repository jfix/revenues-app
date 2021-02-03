<script>
    import { Bar } from "svelte-chartjs";
    import { Card } from "sveltestrap";
    import moment from "moment";
    import { stringify } from 'flatted'

    export let data
    let chartData, chartOptions

    const prepareDataValues = (inData) => {
        let outData = []
        inData.forEach(o => {
            outData = [...outData, Object.values(o)[0]]
        })
        return outData;
    };

    const prepareDataLabels = (inData) => {
        let outData = []
        inData.forEach(o => {
            outData = [...outData, moment(Object.keys(o)[0], "YYYY-MM")]
        })
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
                },
            ],
        })

    $: data && (chartOptions = (type) => {
        return {
        responsive: true,
        maintainAspectRatio: false,
        layout: {
            padding: 5
        },
        tooltips: {
            callbacks: {
                label: function(tooltipItem, data) {
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
    })
</script>

<div class="mt-4">
    <h3>All-time by month</h3>

    <Card class="mt-4 border border-secondary">
        <div class="card-header">iLibrary sales over time</div>
        <div class="card-body" style="height: 354px">
            
            <Bar data={chartData} options={chartOptions()}/>

        </div>
    </Card>
</div>