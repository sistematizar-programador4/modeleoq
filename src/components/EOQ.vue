<template>
  <v-form>  
    <v-container>
      <v-data-table 
        :headers="headers"
        :items="items"
        class="elevation-1"
        prev-icon="mdi-menu-left"
        next-icon="mdi-menu-right"
        sort-icon="mdi-menu-down"
      >
        <template v-slot:items="props">
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
        {text: 'Mes',align:'center',value: 'mes'},
        {text: 'Harina',align:'center',value: 'tasaProduccion' },
        {text: 'Costo Orden',align:'center',value: 'demanda' },
        {text: 'Costo Mante.',align:'center',value: 'cantidadProduccion' },
      ],
      items: [],
      meses: ['enero','febrero','marzo','abril','mayo','junio','julio','agosto','septiembre','octubre','noviembre','diciembre'],
      datosTemporadaValle: datos_temporada_valle,
      datosTemporadaAlta: datos_temporada_alta
    }),
    methods: {
      initTable(){
        var meses = this.meses;
        var datos = [];
        var valorArroba = 24500;
        var tranporte = 5500;
        for (const key in meses) {
        if(key == 0 || key == 5 || key == 6 || key == 10 || key == 11){
          datos = this.datosTemporadaAlta
        }else{
          datos = this.datosTemporadaValle
        }
        if (meses.hasOwnProperty(key)) {
          const  mes     = meses[key];
          const  demanda = this.getDemanda(datos);
          const  orden   = demanda * valorArroba * transporte;
          const mantener = 250000;
        }
        }
      },
      getSecondRandom(datos) {
        //doing a general getSecondRandom
        console.log(`firstRagetSecondRandomndom: ${datos[0].length}`);
        var position = 0
        var array = datos[0]
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
      getFirtsRandom(datos) {
        //doing a general getFirtsRandom
        console.log(`firstRandom: ${datos.length}`);
        var position = 0
        var array = datos
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
      getDemanda(datos) {
        let positionOutside = this.getFirtsRandom(datos)
        let positionInside = this.getSecondRandom(datos)
        var valueToProduce = 0;
        var target = datos[positionOutside]
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
      modelEOQ(){
       
      }
    }
  }
</script>

<style>

</style>