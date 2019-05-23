<template>
  <v-form v-model="valid">  
    <v-container fluid>
      <v-layout>
        <v-flex xs12 md4 >
          <v-text-field
            v-model="P"
            label="Tasa de Producción (Unidades por dia)"
            required
          ></v-text-field>
        </v-flex>
        <v-flex xs12 md4>
          <v-text-field
            v-model="D"
            label="Demanda (Unidades por dia)"
            required
          ></v-text-field>
        </v-flex>
        <v-flex xs12 md4>
          <v-text-field
            v-model="Q"
            label="Cantidad de producción (Unidades por dia)"
            required
          ></v-text-field>
        </v-flex>
        <v-flex xs12 md4>
          <v-text-field
            v-model="L"
            label="Tiempo guia (Unidades por dia)"
            required
          ></v-text-field>
        </v-flex>
        <v-flex xs12 md4>
          <v-text-field
            v-model="K"
            label="Costo de organización (Por corrida prod)"
            required
          ></v-text-field>
        </v-flex>
        <v-flex xs12 md4>
          <v-text-field
            v-model="C"
            label="Valor producto (Por Unidad)"
            required
          ></v-text-field>
        </v-flex>
        <v-flex xs12 md4>
          <v-text-field
            v-model="i"
            label="Tasa de transferencia (Unidades por dia)"
            required
          ></v-text-field>
        </v-flex>
      </v-layout>
      <v-flex xs12 md4>
        <v-text-field
          v-model="H"
          label="Costo de conservación"            
        ></v-text-field>
      </v-flex>
      <v-flex xs12 md>
          <v-btn color="warning" @click="modelPOQ()">Generar POQ</v-btn>
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
        P: '',
        D: '',
        Q: '',
        L: '',
        K: '',
        C: '',
        i: '',
        H: '',
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
      modelPOQ(){
        let tasaProduccion = this.P;
        let demanda = this.D;
        let cantidadProduccion = this.Q;
        let tiempoGuia = this.L;
        let costoOrganizacion = this.K;
        let valorProducto = this.C; // costoConservacionMensual
        let tasaTransferencia = this.i;
        // let costoConservacion = this.H; // costo de conservacion por unidad 

        let costoConservacion = tasaTransferencia * valorProducto;
        this.H = costoConservacion;
        let tiempoCiclo = cantidadProduccion/tasaProduccion; // T = Q/D
        let valMaxInventario = (tasaProduccion - demanda) * tiempoCiclo; // 2. (P-D)*Q/P

        let costoOrganizacionMensual = costoOrganizacion * (demanda/cantidadProduccion); // costo de organizacion mensual = (K * D/Q)
        let costoConservacionMensual =  (valMaxInventario/2) * costoConservacion; // costo de conservacion mensual = (inventario promedio * costoConservacion)
        let constoMensualTotal = costoOrganizacionMensual * costoConservacionMensual;

        let Qobtima = Math.sqrt( (2*demanda*costoOrganizacion)/(costoConservacion* ((tasaProduccion-demanda)/tasaProduccion) ) );

        console.log(Qobtima);


        // let costo = this.S;
        // let mantenimiento = this.C;
        // let lead = this.L;          
        // let eoq = Math.round(Math.sqrt((2*demanda*costo)/mantenimiento));
        // let demanda_dia = demanda/365;
        // let rop =  Math.round(demanda_dia * lead)
        let data = [valMaxInventario,0,valMaxInventario,0] 
        let axis = [0,tiempoCiclo,(parseInt(tiempoCiclo)+1),tiempoCiclo*2]
        this.chartOptions = {
          xaxis: {
            categories:axis
          }
        };
        this.series = [{
          data: data 
        }]
        // console.log(axis)
        // console.log(data)
      }
    }
  }
</script>

<style>

</style>
