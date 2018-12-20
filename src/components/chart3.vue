<template>
  <div>
    <h2>3dChart Test with Plotly</h2>
    <div ref="repspectre"></div>
    <!-- <h3>Number of spectres</h3> -->
    <!-- <vue-numeric-input v-model="chartSetup.nSpectres" autofocus></vue-numeric-input> -->
  </div>
</template>

<script>
import VueNumericInput from 'vue-numeric-input'

const spectres = [];

// const nSpectres = 20;
// const precision = 1;
// const y_range = 800;
// const specThickness = 6;

export default {
  components: {
    VueNumericInput
  },
  data() {
    return {
      chartSetup: {
        nSpectres: 20,
        precision: 1,
        y_range: 800,
        specThickness: 6
      }
    }
  },
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

        for (let i = 0; i < this.chartSetup.nSpectres; i++) {
          const spectre = { x: [], y: [], z: [] };
          const time_int = [this.chartSetup.specThickness * i + 1, this.chartSetup.specThickness * i + 2];
          json[i].Spectrum.forEach((element, index) => {
            // Filtering only multiples of precision to reduce results
            if (index < this.chartSetup.y_range && index % this.chartSetup.precision === 0) {
              // Inserting two times same because of Plotly format
              spectre.x.push(time_int);
              spectre.y.push([element.frecuencia, element.frecuencia]);
              spectre.z.push([element.vibracion, element.vibracion]);
            }
          });
          spectres.push(spectre);
        }

        this.drawPlot();

      } catch (err) {
        console.error(err);
      }
    },
    generateTrace(x, y, z) {
      const trace = {
        x: x,
        y: y,
        z: z,
        type: 'surface',
        showscale: false,
        // cmin: 1,
        // cmax: 1.5,
        // reversescale: true,
        cauto: true,
        colorscale: [
          // For one color put same color around the scale
          // [0, '#E8F1FB'],
          // [0.25, '#D2E3F8'],
          // [0.375, '#BBD5F4'],
          // [0.5, '#A5C8F0'],
          // [0.625, '#8EBAED'],
          // [0.75, '#77ACE9'],
          // [0.875, '#619EE6'],
          // [1, '#4A90E2']
          [0, 'rgb(88,142,202)'],
          [0.1, 'rgb(88,142,202)'],
          [0.2, 'rgb(88,142,202)'],
          [0.3, 'rgb(88,142,202)'],
          [0.4, 'rgb(88,142,202)'],
          [0.5, 'rgb(88,142,202)'],
          [0.6, 'rgb(88,142,202)'],
          [0.7, 'rgb(88,142,202)'],
          [0.8, 'rgb(88,142,202)'],
          [0.9, 'rgb(88,142,202)'],
          [1, 'rgb(88,142,202)'],
        ]
      }

      return trace;
    },
    drawPlot() {
      const data = [];
      const nTraces = this.chartSetup.nSpectres;

      for (let i = 0; i < nTraces; i++) {
        data.push(this.generateTrace(spectres[i].x, spectres[i].y, spectres[i].z));
      }

      console.log('data ', data);

      const layout = {
        showlegend: false,
        autosize: true,
        width: 1092,
        height: 800,
        paper_bgcolor: 'rgb(53, 57, 67)',
        font: {
          size: 12,
          color: 'rgb(176, 176, 176)',
          // family: 'TitilliumWeb-Regular'
        },
        scene: {
          aspectratio: {
            y: 2,
            x: 1,
            z: 1,
          },
          // domain: {
          //   x: [0.33, 0.66],
          //   y: [0.5, 1.0]
          // },
          camera: {
            center: {
              x: 0,
              y: 0,
              z: -0.3
            },
            // Rotation
            eye: {
              x: 1.9,
              y: 0.2,
              z: 0.1
            },
            up: {
              x: 0,
              y: 0,
              z: 0
            }
          },
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
