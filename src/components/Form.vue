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
          <td class="text-xs-center">{{ props.item.demanda }}</td>
          <td class="text-xs-center">{{ props.item.costo }}</td>
          <td class="text-xs-center">{{ props.item.mantenimiento }}</td>
          <td class="text-xs-center">{{ props.item.lead }}</td>
          <td class="text-xs-center">{{ props.item.eoq }}</td>
          <td class="text-xs-center">{{ props.item.rop }}</td>
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
        {text: 'Demanda',align:'center',value: 'demanda' },
        {text: 'Costo',align:'center',value: 'costo' },
        {text: 'Costo Mant.',align:'center',value: 'mantenimiento' },
        {text: 'Lead Time',align:'center',value: 'lead' },
        {text: 'EOQ',align:'center',value: 'eoq' },
        {text: 'ROP',align:'center',value: 'rop' },
      ],
      items: [],
      number:0
    }),
    methods: {
      initTable(){
        console.log("hola");
        for (let index = 0; index < this.number; index++){
          var item = this.modelEOQ()
          this.items.push(item);
          console.log(item);
        }
      },
      modelEOQ(){
        var demanda = Math.round(Math.random() * 1000);
        var costo = Math.round(Math.random() * 100);
        var mantenimiento = Math.round(Math.random() * 10);
        var lead = Math.round(Math.random() * 10);
        var eoq = Math.round(Math.sqrt((2*demanda*costo)/mantenimiento));
        var demanda_dia = demanda/365;
        var rop =  Math.round(demanda_dia * lead)
        var array = {demanda:demanda,costo:costo,mantenimiento:mantenimiento,lead:lead,eoq:eoq,rop:rop}
        console.log(array);
        return array;
      }
    }
  }
</script>

<style>

</style>
