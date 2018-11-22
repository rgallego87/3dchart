<template>
    <div>
        <h2>3dChart Test</h2>
        <Plot
          :data="data"
          :layout="layout"
          class="center"
        />
        <button @click="getSpectrum">Test JSON from server</button>
    </div>
</template>

<script>
import Plot from 'vue-plotly.js';

const y_freq  = [];
const z_vibra = [];

export default {
  name: 'chart',  
  components: { Plot },
  data: () => ({
    data: [
      {
        type: 'surface',
        // y: y_freq,
        z: [z_vibra],
        showscale: false
      }
    ],
    layout: {
      width: 640,
      height: 480,      
    },
  }),
  methods: {
    getSpectrum: async function () {
      try {
        const data = await fetch('http://predictivepumpsapi.azurewebsites.net/api/Spectrum/GetSpectrum/RPG/GA-859-S/3V/2018-11-14%2015%3A33%3A37.383');
        const json = await data.json();  

        json.Spectrum.forEach(element => {
          y_freq.push(element.frecuencia);
          z_vibra.push(element.vibracion);
        });       
        
        console.log(z_vibra);

      } catch (err) {
        console.error(err);
      }
    }
  },  
};
</script>

