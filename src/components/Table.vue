<template>
  <v-form>  
    <v-container fluid>
      <v-data-table 
        dark      
        :headers="headers"
        :items="items"
        class="elevation-1"
        prev-icon="mdi-menu-left"
        next-icon="mdi-menu-right"
        sort-icon="mdi-menu-down"
      >
        <template v-slot:items="props">
          <td class="text-xs-center">ITEM</td>
          <td class="text-xs-center">{{ props.item.tasaProduccion }}</td>
          <td class="text-xs-center">{{ props.item.demanda }}</td>
          <td class="text-xs-center">{{ props.item.cantidadProduccion }}</td>
          <td class="text-xs-center">{{ props.item.tiempoGuia }}</td>
          <td class="text-xs-center">{{ props.item.costoOrganizacionK }}</td>
          <td class="text-xs-center">{{ props.item.valorProducto }}</td>
          <td class="text-xs-center">{{ props.item.tasaTransferencia }}</td>
          <td class="text-xs-center">{{ props.item.costoConservacionH }}</td>
          <td class="text-xs-center">{{ props.item.tiempoCiclo }}</td>
          <td class="text-xs-center">{{ props.item.costoOrganizacionMes }}</td>
          <td class="text-xs-center">{{ props.item.costoConservacionMes }}</td>
          <td class="text-xs-center">{{ props.item.costoMesTotal }}</td>
          <!-- <td class="text-xs-center">{{ props.item.rop }}</td> -->
        </template>
      </v-data-table>
    </v-container>
    <v-container>
      <v-layout wrap justify-space-between>
        <v-flex xs12 md3>
          <v-text-field
            v-model="number"
            label="N° ITEMS"
          ></v-text-field>
        </v-flex>
        <v-flex xs12 md9>
          <v-btn @click="initTable">Success</v-btn>
        </v-flex>
      </v-layout>
    </v-container>
  </v-form>
</template>

<script>

import {datos_temporada_valle,datos_temporada_alta} from '../constants/datos2'

  export default {
    data: () => ({
      headers: [
        {text: 'Item',align:'center',value: 'name'},
        {text: 'Tasa de Producción (P)',align:'center',value: 'tasaProduccion' },
        {text: 'Demanda (D)',align:'center',value: 'demanda' },
        {text: 'Cantidad de producción (Q)',align:'center',value: 'cantidadProduccion' },
        {text: 'Tiempo guia (L)',align:'center',value: 'tiempoGuia' },
        {text: 'Costo de organización (K)',align:'center',value: 'costoOrganizacionK' },
        {text: 'Valor producto (C)',align:'center',value: 'valorProducto' },
        {text: 'Tasa de transferencia (i)',align:'center',value: 'tasaTransferencia' },
        {text: 'Costo de conservación Mensual X unid (H)',align:'center',value: 'costoConservacionH' },
        {text: 'Tiempo de ciclo (T)',align:'center',value: 'tiempoCiclo' },
        {text: 'Costo de organizacion Mensual',align:'center',value: 'costoOrganizacionMes' },
        {text: 'Costo de conservación Mensual',align:'center',value: 'costoConservacionMes' },
        {text: 'Costo Mensual Total',align:'center',value: 'costoMesTotal' }
      ],
      items: [],
      number:0,
      arrayTasaProduccion1: datos_temporada_valle,
      arrayTasaProduccion2: datos_temporada_alta
    }),
    methods: {
      initTable(){
        console.log(this.arrayTasaProduccion1);
        console.log(this.arrayTasaProduccion2);
      },
      getSecondRandom() {
        //doing a general getSecondRandom
        console.log(`firstRagetSecondRandomndom: ${this.arrayTasaProduccion[0].length}`);
        var position = 0
        var array = this.arrayTasaProduccion[0]
        let globalSize = array.length        
        let range = 1/globalSize                
        let firstRandom = Math.random()
        let initRange, finalRange, next        
        for (var index in array) {
          let key = parseInt(index)         
          if (key == 0){
            if (firstRandom >= key && firstRandom < range){          
              position = key
              console.log(`key inside 0 ->: ${key}`);     
            }
          } else if (key > 0){          
            if( key == 1 ){
              initRange = range
              next = key + 1
              finalRange = initRange * next              
            } else {               
              initRange = finalRange
              next = range * (key + 1)
              finalRange = next
            }
            if (firstRandom >= initRange && firstRandom <= finalRange) {          
              position = key
              console.log(`key inside->: ${key}`);
              break;             
            }            
          }                          
        } 
        return position 

      },
      getFirtsRandom() {
        //doing a general getFirtsRandom
        console.log(`firstRandom: ${this.arrayTasaProduccion.length}`);
        var position = 0
        var array = this.arrayTasaProduccion
        let globalSize = array.length
        let range = 1/globalSize                
        let firstRandom = Math.random()
        let initRange, finalRange, next        
        for (var index in array) {
          let key = parseInt(index)       
          if (key == 0){
            if (firstRandom >= key && firstRandom < range){          
              position = key
              console.log(`key 0 ->: ${key}`);   
            }
          } else if (key > 0){          
            if( key == 1 ){
              initRange = range
              next = key + 1
              finalRange = initRange * next              
            } else {               
              initRange = finalRange
              next = range * (key + 1)
              finalRange = next
            }
            if (firstRandom >= initRange && firstRandom <= finalRange) {          
              position = key
              console.log(`key->: ${key}`);         
              break;             
            }
          }                          
        }
        return position
      },
      getTasaProduccion() {
        let positionOutside = this.getFirtsRandom()
        let positionInside = this.getSecondRandom()
        var valueToProduce = 0;
        var target = this.arrayTasaProduccion[positionOutside]
        for (var key in target) {
          if(positionInside == key){
            if(target.hasOwnProperty(key)){            
              valueToProduce = target[key];
              break;
            }
          }
        }        
        return valueToProduce      
      },
      modelPOQ(){
                // this.arrayTasaProduccion[0].forEach(element => {
        //   console.log(JSON.stringify(element, null, 2));
        // });        
        let tasaProduccion = this.getTasaProduccion()
        console.log(`tasaProduccion: ${tasaProduccion}`)
        // let tasaProduccion = getTasaProduccion() ---this.P;
        // let demanda = this.D;
        // let cantidadProduccion = this.Q;
        // let tiempoGuia = this.L;
        // let costoOrganizacion = this.K;
        // let valorProducto = this.C; // costoConservacionMensual
        // let tasaTransferencia = this.i;
        // let costoConservacion = tasaTransferencia * valorProducto;
        // this.H = costoConservacion;
        // let tiempoCiclo = cantidadProduccion/tasaProduccion; // T = Q/D
        // let valMaxInventario = (tasaProduccion - demanda) * tiempoCiclo; // 2. (P-D)*Q/P
        // let costoOrganizacionMensual = costoOrganizacion * (demanda/cantidadProduccion); // costo de organizacion mensual = (K * D/Q)
        // let costoConservacionMensual =  (valMaxInventario/2) * costoConservacion; // costo de conservacion mensual = (inventario promedio * costoConservacion)
        // let constoMensualTotal = costoOrganizacionMensual * costoConservacionMensual;
        // let Qobtima = Math.sqrt( (2*demanda*costoOrganizacion)/(costoConservacion* ((tasaProduccion-demanda)/tasaProduccion) ) );
        // var demanda = Math.round((window.crypto.getRandomValues(new Uint32Array(1))[0] / 4294967295) * 1000);
        // var costo = Math.round((window.crypto.getRandomValues(new Uint32Array(1))[0] / 4294967295) * 100);
        // var mantenimiento = Math.round((window.crypto.getRandomValues(new Uint32Array(1))[0] / 4294967295) * 10);
        // var lead = Math.round((window.crypto.getRandomValues(new Uint32Array(1))[0] / 4294967295) * 10);
        // var eoq = Math.round(Math.sqrt((2*demanda*costo)/mantenimiento));
        // var demanda_dia = demanda/365;
        // var rop =  Math.round(demanda_dia * lead)
        // var array = {demanda:demanda,costo:costo,mantenimiento:mantenimiento,lead:lead,eoq:eoq,rop:rop}
        // console.log(array);
        // var array = {tasaProduccion: tasaProduccion}
        // return array;
      }
    }
  }
</script>

<style>

</style>