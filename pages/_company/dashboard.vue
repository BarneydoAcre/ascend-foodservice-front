<template>
    <div>
        <h1>Dashboard {{ $route.params.company}}</h1>
        <v-card class="">
            <v-card-title>Vendas</v-card-title>
            <v-card-text>
                <div id="graph_chart" class="rounded-lg"></div>
            </v-card-text>
        </v-card>
        
    </div>
</template>

<script>
    export default {
        name: 'dashboard',
        data () {
            return {
                fab: false,
                theme: {
                    txColor: {
                        dark: '#1b2559',
                        light: '#fff',
                    },
                    pallete: [
                        '#c438ef',
                        '#ff409a',
                        '#6452ff',
                        '#05cd99',
                        '#6452ff',
                        '#ffc086',
                        '#4318ff',
                        '#05cd99',
                    ]
                },
            }
        },
        mounted () {
            this.graph()
        },
        methods: {
            heightDefine (h) {
                let height = 400
                // if (window.screen.width < 630) {
                height = parseInt(window.screen.height)*(h/100)
                // height = parseInt(window.screen.height)*h
                return Math.round(height)
            },
            async graph () {
                const req = await fetch(process.env.HOST_BACK+`/dashboard/sales/${this.$route.params.company}`, {
                    method: 'GET'
                })
                if (req.status == 200) {
                    const res = await req.json()
                    console.log(res)
                    console.log(res.map((r) => {
                        return r.mes
                    }))
                    console.log(res.map((r) => {
                        return r.total_venda
                    }))
                    const graph = Highcharts.chart('graph_chart', {
                    plotOptions: {
                            series: {
                                // general options for all series
                            },
                            spline: {
                                color: 'rgb(67, 24, 255)',
                                colorIndex: 'rgb(134, 140, 255)',
                            },
                        },
                        chart: {
                            height: this.heightDefine(35)
                        },
                        title: {
                            text: undefined,
                        },
                        xAxis: {
                            categories: res.map((r) => {
                                return r.dia + '/' + r.mes
                            }),
                        },
                        yAxis: {
                            title: {
                                text: undefined,
                            }
                        },
                        series: [{
                            type: 'spline',
                            name: 'Meses',
                            data: res.map((r) => {
                                return r.total_venda
                            })
                        }]
                    })
                }
            }
        },  
    }
</script>