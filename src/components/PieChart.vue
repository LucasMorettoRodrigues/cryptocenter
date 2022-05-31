<template>
    <div id="chart">
        <apexchart type="donut" :options="chartOptions" :series="series"></apexchart>
    </div>
</template>

<script>
export default {
    props: {
        serie: {
            type: Array
        },
        label: {
            type: Array
        },
    },
    data: function () {
        return {
            chartOptions: {
                chart: {
                    type: 'donut',
                },
                labels: this.label,
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
            series: this.serie
        };
    },
    watch: {
        serie() {
            this.series = this.serie
        },
        label() {
            this.chartOptions = { ...this.chartOptions, labels: this.label }
        }
    }
};
</script>

<style scoped>
#chart {
    width: 550px;
}
</style>
