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
          <td class="text-xs-center">{{ props.item.constoMensualTotal }}</td>
          <td class="text-xs-center">{{ props.item.valMaxInventario }}</td>
          <td class="text-xs-center">{{ props.item.qOptima }}</td>
          <td class="text-xs-center">{{ props.item.prompedidosxPeriodo }}</td>
          <td class="text-xs-center">{{ props.item.tiempoCicloOptimo }}</td>
          <td class="text-xs-center">{{ props.item.tiempoProduccionPrevia }}</td>
          <td class="text-xs-center">{{ props.item.puntoNuevoPedido }}</td>           
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
        {text: 'Costo Mensual Total',align:'center',value: 'constoMensualTotal' },
        {text: 'Valor maximo Inventario',align:'center',value: 'valMaxInventario' },
        {text: 'Cantidad Produccion Optima (Q*)',align:'center',value: 'qOptima' },
        {text: 'N° Promedio pedidios x periodo',align:'center',value: 'prompedidosxPeriodo' },
        {text: 'Tiempo de ciclo Optimo',align:'center',value: 'tiempoCicloOptimo' } ,       
        {text: 'Tiempo finalizacion produccion previa',align:'center',value: 'tiempoProduccionPrevia' },
        {text: 'Punto de nuevo pedido (R)',align:'center',value: 'puntoNuevoPedido' }
      ],
      number: 0,
      items: [],      
      arrayTasaProduccionValle: datos_temporada_valle,
      arrayTasaProduccionAlta: datos_temporada_alta,
      array: null,
      tasaTranferencia_i: null,
      costoOrganizacion_k: null,
      positionOutside: null,
      positionInside: null
    }),
    methods: {
      initTable(){
        for (let index = 0; index < this.number; index++){
          var item = this.modelPOQ()
          this.items.push(item)          
        }
      },
      getSecondRandom() {
        //doing a general getSecondRandom        
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
              break;             
            }            
          }                          
        } 
        return position 

      },
      getFirtsRandom() {
        //doing a general getFirtsRandom        
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
          this.tasaTranferencia_i = 1.01
          this.costoOrganizacion_k = 11200000
        } else {
          this.array = this.arrayTasaProduccionAlta
          this.tasaTranferencia_i = 1.02
          this.costoOrganizacion_k = 12600000
        }
      },
      getDataRandom() {
        this.getdataArray()
        this.positionOutside = this.getFirtsRandom()
        this.positionInside = this.getSecondRandom()
        let positionOutside = this.positionOutside
        let positionInside = this.positionInside
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
        for (var key in this.array[0]) {
          let demanda = this.getDataRandom()        
          if ( tasaProduccion  > demanda) {
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
        for (var x=0; x<=10000; x++) {
          let Q = this.getDataRandom()        
          if ( Q  >= demanda && Q  <= tasaProduccion) {
            value = Q            
            break;
          }                     
        };
        return value        
        
      },
      modelPOQ(){     
        let tasaProduccion_ = (this.getDataRandom())
        let demanda_ = (this.getDemanda(tasaProduccion_))
        let cantidadProduccion_ = (this.getCantidadProduccion(tasaProduccion_, demanda_))

        let tasaProduccion = (tasaProduccion_ * 24).toFixed(4)
        let demanda = (demanda_ * 24).toFixed(4)
        let cantidadProduccion = (cantidadProduccion_ * 24).toFixed(4)


        let tiempoGuia = 0.87  // PREGUNTAR --------------->>>
        let costoOrganizacionK = this.costoOrganizacion_k// PREGUNTAR         
        let valorProducto = 200 
        let tasaTransferencia = this.tasaTranferencia_i // SEGURO?
        let costoConservacionH = tasaTransferencia * valorProducto // POR ITEM AL MES        
        let tiempoCiclo = (cantidadProduccion/tasaProduccion).toFixed(4) // T = Q/D        
        let valMaxInventario = (tasaProduccion - demanda) * tiempoCiclo // 2. (P-D)*Q/P // no se muestra en la tabla        

        let costoOrganizacionMes = Math.round( costoOrganizacionK * (demanda/cantidadProduccion) )  // costo de organizacion mensual = (K * D/Q)        
        let costoConservacionMes =  ( (valMaxInventario/2) * costoConservacionH ).toFixed(4) // costo de conservacion mensual = (inventario promedio * costoConservacionH)
        let constoMensualTotal = Math.round( costoOrganizacionMes * costoConservacionMes )



        let Optima = ((2 * demanda * costoOrganizacionK) / (costoConservacionH * ( (tasaProduccion-demanda)/tasaProduccion) ))
        let qOptima = Math.round( Math.sqrt(Optima) ) 
        // AL SER TAN CARO EL COSTO DE ORGANIZACION DEBO PRODUCIR MAYOR CANTIDAD DEL PRODUCTO        

        let prompedidosxPeriodo = (demanda/qOptima).toFixed(4)

        //  DETERMINACION DE PUNTO DE NUEVO PEDIDO
        let tiempoCicloOptimo = (qOptima/demanda).toFixed(4)
        let tiempoProduccionPrevia = (qOptima/tasaProduccion).toFixed(4)
        let puntoNuevoPedido = (tiempoGuia * demanda).toFixed(4)

        var arrayItem = {
          tasaProduccion: tasaProduccion,
          demanda: demanda,
          cantidadProduccion: cantidadProduccion,
          tiempoGuia: tiempoGuia,
          costoOrganizacionK: costoOrganizacionK,
          valorProducto: valorProducto,
          tasaTransferencia: tasaTransferencia,
          costoConservacionH: costoConservacionH,
          tiempoCiclo: tiempoCiclo,
          costoOrganizacionMes: costoOrganizacionMes,
          costoConservacionMes: costoConservacionMes,
          constoMensualTotal: constoMensualTotal,
          valMaxInventario: valMaxInventario,
          qOptima: qOptima,
          prompedidosxPeriodo: prompedidosxPeriodo,
          tiempoCicloOptimo: tiempoCicloOptimo,
          tiempoProduccionPrevia: tiempoProduccionPrevia,
          puntoNuevoPedido: puntoNuevoPedido
        }
        return arrayItem;
      }
    }
  }
</script>

<style>

</style>