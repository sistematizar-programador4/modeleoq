<template>
  <v-form v-model="valid">  
    <v-container>
      <v-layout>
        <v-flex xs12 md4 >
          <v-text-field
            v-model="R"
            label="Demanda (Unidades por año)"
            required
          ></v-text-field>
        </v-flex>

        <v-flex xs12 md4>
          <v-text-field
            v-model="S"
            label="Costo de emitir una orden"
            required
          ></v-text-field>
        </v-flex>

        <v-flex xs12 md4>
          <v-text-field
            v-model="C"
            label="Costo de mantener en inventario"
            required
          ></v-text-field>
        </v-flex>
        <v-flex xs12 md4>
          <v-text-field
            v-model="L"
            label="Lead Time"
            required
          ></v-text-field>
        </v-flex>
      </v-layout>
    <v-flex xs12 md>
        <v-btn color="warning" @click="modelEOQ()">Generar EOQ</v-btn>
    </v-flex>
    </v-container>
    <v-container>
        <apexchart width="550" type="line" :options="chartOptions" :series="series"></apexchart>
    </v-container>
  </v-form>
</template>

<script>
  export default {
    data: () => ({
      valid: false,
        R: '',
        S: '',
        C: '',
        L: '',
        chartOptions: {
            chart: {
                id: 'vuechart-example',
            },
            xaxis: {
                categories: [1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998],
            },
        },
        series: [{
            name: 'series-1',
            data: [30, 40, 45, 50, 49, 60, 70, 81]
        }]
    }),

    methods: {
        modelEOQ(){
            var demanda = this.R;
            var costo = this.S;
            var mantenimiento = this.C;
            var lead = this.L;
            var eoq = Math.round(Math.sqrt((2*demanda*costo)/mantenimiento));
            var demanda_dia = demanda/365;
            var rop =  Math.round(demanda_dia * lead)
            var data = [eoq,0,eoq,0] 
            var axis = [0,lead,(parseInt(lead)+1),lead*2]
            this.chartOptions = {
                xaxis: {
                    categories:axis
                }
            };
            this.series = [{
            data: data
            }]
            console.log(axis)
            console.log(data)
        }
    }
  }
</script>

<style>

</style>
