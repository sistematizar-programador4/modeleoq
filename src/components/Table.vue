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
        // {text: 'Lead Time',align:'center',value: 'lead' },
        // {text: 'EOQ',align:'center',value: 'eoq' },
        // {text: 'ROP',align:'center',value: 'rop' },
      ],
      items: [],
      number:0
    }),
    methods: {
      initTable(){
        for (let index = 0; index < this.number; index++){
          var item = this.modelEOQ()
          this.items.push(item);
          // console.log(item);
        }
      },
      modelEOQ(){
        // var demanda = Math.round((window.crypto.getRandomValues(new Uint32Array(1))[0] / 4294967295) * 1000);
        // var costo = Math.round((window.crypto.getRandomValues(new Uint32Array(1))[0] / 4294967295) * 100);
        // var mantenimiento = Math.round((window.crypto.getRandomValues(new Uint32Array(1))[0] / 4294967295) * 10);
        // var lead = Math.round((window.crypto.getRandomValues(new Uint32Array(1))[0] / 4294967295) * 10);
        // var eoq = Math.round(Math.sqrt((2*demanda*costo)/mantenimiento));
        // var demanda_dia = demanda/365;
        // var rop =  Math.round(demanda_dia * lead)
        // var array = {demanda:demanda,costo:costo,mantenimiento:mantenimiento,lead:lead,eoq:eoq,rop:rop}
        // console.log(array);
        // return array;
      }
    }
  }
</script>

<style>

</style>

