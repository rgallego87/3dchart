<template>
    <div>
        <h2>3dChart Test with Plotly</h2>
        <div ref="repspectre"></div>
    </div>
</template>

<script>
import Plotly from 'plotly.js';

const y_freq = [];
const z_vibra = [];

export default {
    name: 'chart2',

    created() {
        this.fetchData()
    },

    methods: {
        fetchData() {
            Plotly.d3.csv('https://raw.githubusercontent.com/plotly/datasets/master/api_docs/mt_bruno_elevation.csv', (err, rows) => {
                if (err) {
                    rows = []
                }
                this.rows = rows
                this.drawPlot()
            })
        },
        drawPlot() {
            let rows = this.rows
            function unpack(rows, key) {
                return rows.map(function (row) { return row[key]; });
            }
            var z_data = []
            for (var i = 0; i < 24; i++) {
                z_data.push(unpack(rows, i));
            }

            var data = [{
                z: z_data,
                type: 'surface'
            }];

            var layout = {
                title: 'Mt Bruno Elevation',
                autosize: false,
                width: 500,
                height: 500,
                margin: {
                    l: 65,
                    r: 50,
                    b: 65,
                    t: 90,
                }
            };
            Plotly.newPlot(this.$refs.repspectre, data, layout);
        }
    }
}

</script>

