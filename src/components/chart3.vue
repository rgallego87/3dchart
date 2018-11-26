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

const nSpectres = 20;
const precision = 25;
const y_range = 800;

let x_fake = 1;
const x_times = [];
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
                // const data = await fetch('http://predictivepumpsapi.azurewebsites.net/api/Spectrum/GetSpectrumByDates/RPG/GA-859-S/1V/2018-10-11%2000%3A08%3A18.907/2018-10-11%2003%3A43%3A16.433');
                // const json = await data.json();     
                const json = await require('../../public/chart-test.json');

                // console.log(json);
                // this.x_time = Date.parse(json.SourceTimeStamp);

                // Quantity of plots to draw with i (performance)
                for (let i = 0; i < nSpectres; i++) {
                    x_fake += 2;
                    json[i].Spectrum.forEach((element, index) => {
                        // Filtering only multiples of precision to reduce results
                        if (index < y_range && index % precision === 0) {
                            // Inserting two times same because of Plotly                                                     
                            x_times.push(new Array(x_fake, x_fake + 1));
                            y_freqs.push(new Array(element.frecuencia, element.frecuencia));
                            z_vibras.push(new Array(element.vibracion, element.vibracion));
                        }
                    });
                }

                console.log('x_times', x_times);
                console.log('y_freqs', y_freqs);
                console.log('z_vibras', z_vibras);

                await this.drawPlot();

            } catch (err) {
                console.error(err);
            }
        },
        generateTrace(x, y, z) {
            let trace = {
                x: x,
                y: y,
                z: z,
                // name: `spectre${x_fake}`,
                type: 'surface',
                autocolorscale: true,
                showscale: false
            }

            return trace;
        },
        drawPlot() {

            let data = [];
            let nTraces = nSpectres;
            for (let i = 0; i < nTraces; i++) {
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
                    yaxis: {
                        title: 'Frequency',
                        // range: [0, 1000]
                    },
                    zaxis: { title: 'Vibration' }
                }
            };
            Plotly.newPlot(this.$refs.repspectre, data, layout);
        }
    }
}

</script>



