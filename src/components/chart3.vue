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
const precision = 1;
const y_range = 2000;

const spectres = []
// const x_times = [];
// const y_freqs = [];
// const z_vibras = [];

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
          const spectre = { x: [], y: [], z: [] };
          const time_int = [2 * i + 1, 2 * i + 2];
          json[i].Spectrum.forEach((element, index) => {
            // Filtering only multiples of precision to reduce results
            if (index < y_range && index % precision === 0) {
              // Inserting two times same because of Plotly
              spectre.x.push(time_int);
              spectre.y.push([element.frecuencia, element.frecuencia]);
              spectre.z.push([element.vibracion, element.vibracion]);
            }
          });
          // spectre.x = this.generateX(spectres.length, spectre.y.length);
          spectres.push(spectre);
        }

        // console.log('x_times', x_times);
        // console.log('y_freqs', y_freqs);
        // console.log('z_vibras', z_vibras);

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
        // name: `spectre${x_fake}`,
        type: 'surface',
        showscale: false,
        colorscale: [
          [0, '#E8F1FB'],
          [0.25, '#D2E3F8'],
          [0.375, '#BBD5F4'],
          [0.5, '#A5C8F0'],
          [0.625, '#8EBAED'],
          [0.75, '#77ACE9'],
          [0.875, '#619EE6'],
          [1, '#4A90E2']
        ]
      }

      return trace;
    },
    // generateX(n,m) {      
    //   const x_times = [];


    //   for (let i = 0; i < m; i++) {
    //     x_times.push(z);
    //   }

    //   return x_times;
    // },
    drawPlot() {

      let data = [];
      let nTraces = nSpectres;
      // let x_fin = 1;

      for (let i = 0; i < nTraces; i++) {
        data.push(this.generateTrace(spectres[i].x, spectres[i].y, spectres[i].z));
      }

      console.log('data ', data);

      let layout = {
        showlegend: false,
        autosize: true,
        width: 600,
        height: 600,
        paper_bgcolor: 'rgb(53, 57, 67)',
        font: {
          size: 12,
          color: 'rgb(176, 176, 176)',
          // family: 'TitilliumWeb-Regular'
        },
        scene: {
          xaxis: {
            title: 'Time',
            gridcolor: 'rgb(86, 99, 105)'
          },
          yaxis: {
            title: 'Frequency',
            gridcolor: 'rgb(86, 99, 105)'
            // range: [0, 1000]
          },
          zaxis: {
            title: 'Vibration',
            gridcolor: 'rgb(86, 99, 105)'
          }
        }
      };
      Plotly.newPlot(this.$refs.repspectre, data, layout);
    }
  }
}

</script>



