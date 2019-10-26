<template>
  <div >
    <!-- <p >{{msg}}</p> -->
<!-- 
  <v-container>
      <v-row>
        <v-col>
            <p>Hello</p>
        </v-col>
      </v-row>
  </v-container> -->
    <v-container>
      <v-data-table :headers="mainHeaders"
                     :items="mainItems"
                     item-key="name" items-per-page="5">
        <template slot="items">
          <tr @click="props.expanded = !props.expanded">
            <td class="text-xs">{{ mainItems.name }}</td>
            <td class="text-xs">{{ mainItems.age }}</td>
          </tr>
        </template>
      </v-data-table>
    </v-container>  
  </div>
</template>
<script>
// import HelloWorld from './components/HelloWorld';
export default {
  name: 'ContactTable',
  props: {
    msg: String
  },
  data () {
      return {
        mainHeaders: [
          { text: 'Name', value: 'name' },
          { text: 'Age', value: 'age' }
        ],
        subHeaders: [
          { text: 'Color', value: 'color' },
          { text: 'Value', value: 'value' }
        ],
        mainItems: [
          { name: 'Marc Moreno', age: '30' },
          { name: 'Wallace  Frank', age: '31' },
          { name: 'Enrique  Sanders', age: '45' },
          { name: 'Alyssa Butler', age: '23' },
          { name: 'Domingo  Gill', age: '29' },
          { name: 'Felix  May', age: '28' },
          { name: 'Bradford Powell', age: '15' },
          { name: 'Hattie Coleman', age: '17' },
          { name: 'Marty  Jordan', age: '23' },
          { name: 'Bobbie Webb', age: '28' },
          { name: 'Marc Moreno', age: '30' },
          { name: 'Wallace  Frank', age: '31' },
          { name: 'Enrique  Sanders', age: '45' },
          { name: 'Alyssa Butler', age: '23' },
          { name: 'Domingo  Gill', age: '29' },
          { name: 'Felix  May', age: '28' },
          { name: 'Bradford Powell', age: '15' },
          { name: 'Hattie Coleman', age: '17' },
          { name: 'Marty  Jordan', age: '23' },
          { name: 'Bobbie Webb', age: '28' },
          { name: 'Marc Moreno', age: '30' },
          { name: 'Wallace  Frank', age: '31' },
          { name: 'Enrique  Sanders', age: '45' },
          { name: 'Alyssa Butler', age: '23' },
          { name: 'Domingo  Gill', age: '29' },
          { name: 'Felix  May', age: '28' },
          { name: 'Bradford Powell', age: '15' },
          { name: 'Hattie Coleman', age: '17' },
          { name: 'Marty  Jordan', age: '23' },
          { name: 'Bobbie Webb', age: '28' }
        ],
        subItems: [
          { color: 'Red', value: '1' },
          { color: 'Green', value: '2' },
          { color: 'Blue', value: '3' },
          { color: 'Black', value: '4' }
        ]
      }
    }
};
</script>
<style>
  .v-data-table{
    max-height: 100%;
  }
</style>