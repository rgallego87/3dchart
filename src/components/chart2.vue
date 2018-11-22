<template>
    <div>
        <h2>3dChart Test with Plotly</h2>
        <div ref="repspectre"></div>
    </div>
</template>

<script>
// import Plotly from 'plotly.js';
// var Plotly = require('plotly.js/lib/core');
// Plotly.register(require('plotly.js/lib/surface'));

const y_freq = [];
const z_vibra = [];

export default {
    name: 'chart2',

    created() {
        this.fetchData()
    },

    methods: {
        async fetchData() {
            try {
                const data = await fetch('http://predictivepumpsapi.azurewebsites.net/api/Spectrum/GetSpectrum/RPG/GA-859-S/3V/2018-11-14%2015%3A33%3A37.383');
                const json = await data.json();

                json.Spectrum.forEach(element => {
                    y_freq.push(element.frecuencia);
                    z_vibra.push(element.vibracion);
                });

                console.log('y_freq', y_freq)
                console.log('z_vibra', z_vibra)
                this.drawPlot();

            } catch (err) {
                console.error(err);
            }
        },
        drawPlot() {
            var data = [{
                // z: z_data,
                y: y_freq,
                z: [z_vibra, z_vibra],
                type: 'surface'
            }];

            var layout = {

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


