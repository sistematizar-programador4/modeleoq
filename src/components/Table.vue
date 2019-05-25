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
          <v-btn @click="initTable()">Success</v-btn>
        </v-flex>
      </v-layout>
    </v-container>
  </v-form>
</template>

<script>

import {datos_temporada_valle, datos_temporada_alta} from '../constants/datos2'

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
      arrayTasaProduccionValle: datos_temporada_valle,
      arrayTasaProduccionAlta: datos_temporada_alta,
      array: null,
      number: 0
    }),
    methods: {
      initTable(){
        for (let index = 0; index < this.number; index++){
          var item = this.modelPOQ()
          this.items.push(item);
          console.log(`item: ${item}`);
        }
        console.log(this.arrayTasaProduccionValle);
        console.log(this.arrayTasaProduccionAlta);
      },
      getSecondRandom() {
        //doing a general getSecondRandom
        // console.log(`firstRagetSecondRandomndom: ${this.array[0].length}`);
        var position = 0
        var array = this.array[0]
        let globalSize = array.length        
        let range = 1/globalSize                
        let firstRandom = Math.random()
        let initRange, finalRange, next        
        for (var index in array) {
          let key = parseInt(index)         
          if (key == 0){
            if (firstRandom >= key && firstRandom < range){          
              position = key
              // console.log(`key inside 0 ->: ${key}`);     
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
              // console.log(`key inside->: ${key}`);
              break;             
            }            
          }                          
        } 
        return position 

      },
      getFirtsRandom() {
        //doing a general getFirtsRandom
        // console.log(`firstRandom: ${this.array.length}`);
        var position = 0
        var array = this.array
        let globalSize = array.length
        let range = 1/globalSize                
        let firstRandom = Math.random()
        let initRange, finalRange, next        
        for (var index in array) {
          let key = parseInt(index)       
          if (key == 0){
            if (firstRandom >= key && firstRandom < range){          
              position = key
              // console.log(`key 0 ->: ${key}`);   
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
              // console.log(`key->: ${key}`);         
              break;             
            }
          }                          
        }
        return position
      },
      getdataArray() {
        // get array of data valle(7/12) or alta(5/12) 
        let dataRandom = Math.random()
        if (dataRandom >= (7/12)) {
          this.array = this.arrayTasaProduccionValle
        } else {
          this.array = this.arrayTasaProduccionAlta
        }
      },
      getDataRandom() {
        this.getdataArray()
        let positionOutside = this.getFirtsRandom()
        let positionInside = this.getSecondRandom()
        var value = 0;
        var target = this.array[positionOutside]
        for (var key in target) {
          if(positionInside == key){
            if(target.hasOwnProperty(key)){            
              value = target[key];
              break;
            }
          }
        }        
        return value      
      },
      getDemanda (tasaProduccion) {
        let value = 0        
        let demanda = this.getDataRandom()        
        for (var key in this.array[0]) {
          if ( tasaProduccion  >= demanda) {
            value = demanda
            break;
          } else {
            demanda = this.getDataRandom()
            value = demanda
          }                         
        };
        return value
        
      },
      getCantidadProduccion (tasaProduccion, demanda) {
        let value = 0        
        let Q = this.getDataRandom()        
        for (var key in this.array[0]) {
          if ( Q  > demanda && Q  < tasaProduccion) {
            value = Q
            break;
          } else {
            Q = this.getDataRandom()
            value = Q
          }                         
        };
        return value
        
      },
      modelPOQ(){     
        let tasaProduccion = this.getDataRandom()
        let demanda = this.getDemanda(tasaProduccion)   
        let cantidadProduccion = this.getCantidadProduccion(tasaProduccion, demanda)
        // let tiempoGuia =   // PREGUNTAR 
        // let costoOrganizacionK =   // PREGUNTAR 
        let valorProducto = 200 
        let tasaTransferencia = 100 // SEGURO?
        let costoConservacionH = tasaTransferencia * valorProducto // POR ITEM AL MES
        let tiempoCiclo = cantidadProduccion/tasaProduccion; // T = Q/D
        let valMaxInventario = (tasaProduccion - demanda) * tiempoCiclo; // 2. (P-D)*Q/P // no se muestra en la tabla
        // let costoOrganizacionMes = costoOrganizacionK * (demanda/cantidadProduccion); // costo de organizacion mensual = (K * D/Q)
        let costoConservacionMes =  (valMaxInventario/2) * costoConservacionH; // costo de conservacion mensual = (inventario promedio * costoConservacionH)
        // let constoMensualTotal = costoOrganizacionMes * costoConservacionMes;
        console.log(`tasaProduccion: ${tasaProduccion}`)
        console.log(`demanda: ${demanda}`)
        console.log(`cantidadProduccion: ${cantidadProduccion}`)    
        // let tiempoGuia = this.L;                                     TO DO
        // let costoOrganizacion = this.K;                              TO DO
        // let valorProducto = this.C; // costoConservacionMensual      DONE
        // let tasaTransferencia = this.i;                              TO DO
        // let costoConservacion = tasaTransferencia * valorProducto;   TO DO        
        // let tiempoCiclo = cantidadProduccion/tasaProduccion; // T = Q/D    DONE
        var arrayItem = {
          tasaProduccion: tasaProduccion,
          demanda: demanda,
          cantidadProduccion: cantidadProduccion,
          // tiempoGuia: tiempoGuia,
          // costoOrganizacionK: costoOrganizacionK,
          valorProducto: valorProducto,
          tasaTransferencia: tasaTransferencia,
          costoConservacionH: costoConservacionH,
          tiempoCiclo: tiempoCiclo,
          // costoOrganizacionMes: costoOrganizacionMes,
          costoConservacionMes: costoConservacionMes,
          // constoMensualTotal: constoMensualTotal
        }
        return arrayItem;
      }
    }
  }
</script>

<style>

</style>