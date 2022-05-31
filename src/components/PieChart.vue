<template>
    <div id="chart">
        <apexchart type="donut" :options="chartOptions" :series="series"></apexchart>
    </div>
</template>

<script>
export default {
    props: {
        series: {
            type: Array
        },
        labels: {
            type: Array
        },
    },
    data: function () {
        return {
            chartOptions: {
                chart: {
                    type: 'donut',
                },
                labels: this.labels,
                responsive: [{
                    breakpoint: 480,
                    options: {
                        chart: {
                            width: 200
                        },
                        legend: {
                            position: 'bottom'
                        }
                    }
                }]
            },
            series: this.series
        };
    },
};
</script>

<style scoped>
#chart {
    margin: 20px auto;
    width: 500px;
}
</style>
