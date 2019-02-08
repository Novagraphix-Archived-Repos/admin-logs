<template>
    <div>
        <heading class="mb-6">Adminlogs - Dashboard</heading>

        <!-- Three columns -->
        <div class="flex mb-4">
            <div class="w-1/3">
                <doughnut-chart
                    :chart-data="dataCollection"
                    :options="{
                        responsive: true,
                        maintainAspectRatio: true,
                        legend: {
                            display: true,
                            position: 'bottom',
                            labels: {
                                boxWidth: 10,
                                fontSize: 10,
                                fontColor: 'rgb(200, 190, 180)',
                                usePointStyle: true
                            }
                        }
                    }"
                    :height="400">
                </doughnut-chart>
            </div>
            <div>
                <div class="flex flex-wrap mb-4">
                    <div class="w-1/3 p-4" v-for="(percent, key) in percents" :key="key">
                        <div class="card overflow-hidden bg-30" :style="percent.count !== 0 ? 'color: #FFF;background-color :' + percent.backgroundColor : '' ">
                            <div class="px-6 py-4">
                                <div class="font-bold text-xl mb-2"><i :class="percent.icon"></i> {{ percent.name }}</div>
                                <p class="text-grey-lighter text-base">
                                    {{ percent.count }} entries - {{ percent.percent }}%
                                </p>
                                <div class="progress mt-3">
                                    <div class="bar" :style="{ width: percent.percent + '%' }"></div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import DoughnutChart from '../plugins/DoughnutChart'
    import axios from 'axios'

    export default {
        components: {
            DoughnutChart
        },
        data () {
            return {
                dataCollection: {},
                percents: {}
            }
        },
        mounted () {
            this.getChartData()
        },
        methods: {
            getChartData () {
                axios.get('/nova-vendor/novagraphix/nova-log-viewer/get_chart_data')
                    .then(({data}) => {
                        this.dataCollection = data.chartData
                        this.percents = data.percents
                    })
            }
        }
    }
</script>

<style>
    .progress {
        width: 100%;
        background-color: #000;
    }

    .bar {
        height: 5px;
        background-color: #FFF;
    }
</style>
