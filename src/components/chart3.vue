<template>
    <div>
        <h2>3dChart Test with Plotly (20 Plots)</h2>
        <div ref="repspectre"></div>
    </div>
</template>

<script>
// import Plotly from 'plotly.js';
// var Plotly = require('plotly.js/lib/core');
// Plotly.register(require('plotly.js/lib/surface'));

const x_times = [];
let x_fake = 1;
const y_freqs = [];
const z_vibras = [];

export default {
    name: 'chart3',

    created() {
        this.fetchData();
    },

    methods: {
        async fetchData() {
            try {
                const data = await fetch('http://predictivepumpsapi.azurewebsites.net/api/Spectrum/GetSpectrumByDates/RPG/GA-859-S/1V/2018-10-11%2000%3A08%3A18.907/2018-10-11%2003%3A43%3A16.433');
                const json = await data.json();

                // console.log(json);

                // this.x_time = Date.parse(json.SourceTimeStamp);

                // Quantity of plots to draw with i (performance)
                for (let i = 0; i <= 2; i++) {
                    json[i].Spectrum.forEach((element, index) => {
                        // Filtering only multiples of 10 to reduce results
                        if (index % 1000 === 0) {
                            // Inserting two times same
                            // x_times.push(new Array(index, index + 1));                            
                            x_times.push(new Array(x_fake, x_fake + 1));
                            y_freqs.push(new Array(element.frecuencia, element.frecuencia));
                            z_vibras.push(new Array(element.vibracion, element.vibracion));
                            x_fake++;
                        }
                    });
                }

                console.log('x_times', x_times);
                console.log('y_freqs', y_freqs);
                console.log('z_vibras', z_vibras);

                this.drawPlot();

            } catch (err) {
                console.error(err);
            }
        },
        generateTrace(x, y, z) {
            let trace = {
                x: x,
                y: y,
                z: z,
                type: 'surface',
                autocolorscale: true,
                showscale: false
            }

            return trace;
        },
        drawPlot() {

            // var trace1 = {
            //     x: [[2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3], [2, 3]],
            //     // x: x_times,
            //     y: y_freqs,
            //     z: z_vibras,
            //     name: '',
            //     type: 'surface',
            //     autocolorscale: true,
            //     showscale: false
            // }

            // var trace2 = {
            //     x: [[4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5], [4, 5]],
            //     // x: x_times,
            //     y: y_freqs,
            //     z: z_vibras,
            //     name: '',
            //     autocolorscale: true,
            //     type: 'surface',
            //     showscale: false
            // }

            // var trace3 = {
            //     x: [[6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7], [6, 7]],
            //     // x: x_times,
            //     y: y_freqs,
            //     z: z_vibras,
            //     name: '',
            //     autocolorscale: true,
            //     type: 'surface',
            //     showscale: false
            // }

            let data = [];
            for (let i = 0; i < 3; i++) {
                data.push(this.generateTrace(x_times, y_freqs, z_vibras));
            }

            console.log('data ', data);

            let layout = {
                showlegend: false,
                autosize: true,
                width: 600,
                height: 600,
                scene: {
                    xaxis: { title: 'Time' },
                    yaxis: { title: 'Frequency' },
                    zaxis: { title: 'Vibration' }
                }
            };
            Plotly.newPlot(this.$refs.repspectre, data, layout);
        }
    }
}

</script>



