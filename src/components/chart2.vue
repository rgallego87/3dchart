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

const x_time = '';
const y_freq = [];
const z_vibra = [];

export default {
    name: 'chart2',

    created() {
        this.fetchData();
    },

    methods: {
        async fetchData() {
            try {
                const data = await fetch('http://predictivepumpsapi.azurewebsites.net/api/Spectrum/GetSpectrum/RPG/GA-859-S/3V/2018-11-14%2015%3A33%3A37.383');
                // const data = await fetch('http://predictivepumpsapi.azurewebsites.net/api/Spectrum/GetSpectrumByDates/RPG/GA-859-S/1V/2018-10-11%2000%3A08%3A18.907/2018-10-11%2003%3A43%3A16.433');
                const json = await data.json();

                this.x_time = Date.parse(json.SourceTimeStamp);

                json.Spectrum.forEach((element, index) => {
                    // Filtering only multiples of 10 to reduce results
                    if (index % 10 === 0) {
                        y_freq.push(element.frecuencia);
                        z_vibra.push(element.vibracion);
                    }
                });
                // for (let i = 0; i < json.Spectrum.lenght; i++) {
                //     json.Spectrum[i].forEach((element, index) => {
                //         // Filtering only multiples of 10 to reduce results
                //         if (index % 10 === 0) {
                //             y_freq.push(element.frecuencia);
                //             z_vibra.push(element.vibracion);
                //         }
                //     });
                // }

                console.log('x_time', this.x_time);
                console.log('y_freq', y_freq);
                console.log('z_vibra', z_vibra);

                this.drawPlot();

            } catch (err) {
                console.error(err);
            }
        },
        drawPlot() {
            var data = [{
                // x: [this.x_time],
                y: y_freq,
                z: [z_vibra, z_vibra],
                type: 'surface'
            }];

            var layout = {

                autosize: false,
                width: 700,
                height: 700,
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



