<template>
  <v-form>  
    <v-container>
      <v-data-table 
        :headers="headers"
        :items="items"
        class="elevation-1"
        hide-actions
      >
        <template v-slot:items="props">
          <td style="width:118px !important;" class="text-xs-left">{{ props.item.mes }}</td>
          <td style="width:118px !important;" class="text-xs-center">{{ props.item.demanda }}</td>
          <td style="width:118px !important;" class="text-xs-right">{{ new Intl.NumberFormat("en-EN", {style: "currency", currency: "USD"}).format(props.item.orden) }}</td>
          <td style="width:118px !important;" class="text-xs-right">{{ new Intl.NumberFormat("en-EN", {style: "currency", currency: "USD"}).format(props.item.mantener) }}</td>
          <td style="width:118px !important;" class="text-xs-center">{{ props.item.lead }}</td>
        </template>
      </v-data-table>
    </v-container>
    <v-container>
      <v-layout wrap justify-space-between>
        <v-flex xs12 md3>
          <v-text-field
              v-model="demandapromedio"
              label="Demanda Promedio"
          ></v-text-field>
        </v-flex>
        <v-flex xs12 md3>
          <v-text-field 
              v-model="ordenpromedio"
              label="Costo Orden Promedio"
          ></v-text-field>
        </v-flex>
        <v-flex xs12 md3>
          <v-text-field 
              v-model="eoq"
              label="EOQ"
          ></v-text-field>
        </v-flex>
        <v-flex xs12 md3>
          <v-text-field 
              v-model="rop"
              label="ROP"
          ></v-text-field>
        </v-flex>
        <v-flex xs12 md12>
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
        {text: 'Mes',align:'center',value: 'mes',sortable:false},
        {text: 'Harina (@)',align:'center',value: 'tasaProduccion' ,sortable:false},
        {text: 'Costo Orden ($)',align:'center',value: 'demanda' ,sortable:false},
        {text: 'Costo Mante.($)',align:'center',value: 'cantidadProduccion' ,sortable:false},
        {text: 'Lead Time(Dias)',align:'center',value: 'cantidadProduccion' ,sortable:false},
      ],
      items: [],
      mantener:250000,
      lead:1,
      rop:0,
      eoq:0,
      demandapromedio:0,
      ordenpromedio:0,
      meses: ['enero','febrero','marzo','abril','mayo','junio','julio','agosto','septiembre','octubre','noviembre','diciembre'],
      mesesTemporadaAlta: ['enero','junio','julio','noviembre','diciembre'],
      mesesTemporadaValle: ['febrero','marzo','abril','mayo','agosto','septiembre','octubre'],
      datosTemporadaValle: datos_temporada_valle,
      datosTemporadaAlta: datos_temporada_alta
    }),
    methods: {
      initTable(){
        this.items = [];
        var meses = this.meses;
        var valorArroba = 24500;
        var transporte = 5500;
        var totaldemanda = 0;
        var totalorden = 0;
        for (const key in meses) {
          var datos = this.getDataRandom();
          let demanda = this.getDemanda(datos);
          totaldemanda += demanda;
          totalorden +=  Math.round(demanda * (valorArroba + transporte));
          this.items.push({
            mes:meses[key],
            demanda: demanda,
            orden:  Math.round(demanda * (valorArroba + transporte)),
            mantener:250000,
            lead:1
          })
        }
        this.demandapromedio = (totaldemanda / 12).toFixed(1);
        this.ordenpromedio = new Intl.NumberFormat("en-EN", {style: "currency", currency: "USD"}).format(totalorden / 12);
        this.modelEOQ(this.demandapromedio,(totalorden / 12))
        
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
      getDataRandom(){
        let Random = Math.random()
        console.log(Random);
        if(Random <= (7/12)){
          return this.datosTemporadaValle
        }else{
          return this.datosTemporadaAlta
        }
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
      modelEOQ(demandapromedio,costopromedio){
        var demanda = demandapromedio * 12
        var demanda_dia = demanda/31;
        var costo = costopromedio * 12
        var mantenimiento = 250000
        var lead = 1
        this.eoq = Math.sqrt((2*demanda*costo)/mantenimiento);
        this.rop =  demanda_dia*lead;

      }
    }
  }
</script>

<style>

</style>