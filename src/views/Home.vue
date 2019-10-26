<template>
  <div>
    <v-container>
      <v-row>
        <div class="col col-3 sidebar">
          <v-col>
            <Sidebar />
          </v-col>
        </div>
        
        <v-col>
          <v-container>
            <div class="calls">
              <v-row>
                <v-col>
                  <p>Contacts</p>
                  
                  <ContactTable msg="Hello"></ContactTable>
                </v-col>
              </v-row>
            </div>
            
            <div class="calls" align-center>
              
              <v-row >
                <v-col>
                  <!-- <p>Hello</p> -->
                  <p>Calendar</p>
                  <ContactTable msg="Hello"></ContactTable>
                </v-col>
              </v-row>
              
            </div>
          </v-container>
          
        </v-col>
      </v-row>
    </v-container>
  
  <!-- <HelloWorld></HelloWorld> -->
  </div>
</template>
<script>
import ContactTable from '@/components/ContactTable.vue';
import Sidebar from '@/components/Sidebar.vue';
// import HelloWorld from './HelloWorld.vue'
export default {
  name: 'home',
  components: {
    ContactTable,
    Sidebar,
    // HelloWorld
  }
};
</script>
<style>
*{
  max-height: vh;
}
.container{
  margin-left: 0;
  padding: 0;
}
.calls {
  /*height: 400px;*/
  width: 100%;
}
  *, .row, .v-container {
    padding: 0;
    margin: 0;
  }
  .sidebar {
  
    background-image: linear-gradient(to bottom,  #399BE2, #42b0ff);
    color: white;
  }
</style>