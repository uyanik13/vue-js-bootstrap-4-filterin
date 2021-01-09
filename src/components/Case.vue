<template>

  <b-row class="justify-content ml-5 mr-5">
    <b-col m="6" md="5" lg="4" class="mt--5 ">
      <form  @submit.prevent="search">
           <div class="">
            <div class="mt-2 text-left">Name: </div>
              <b-form-input v-model="initialData.name" placeholder="Enter your name" class="mb-2"></b-form-input>
           </div>
           <div>
              <div class="mt-2 text-left">Age: </div>
              <b-form-input v-model="initialData.age" placeholder="Enter your Age" class="mb-2"></b-form-input>
           </div>
           <div>
             <div class="mt-2 text-left">City: </div>
            <multiselect v-model="initialData.city" :options="options" :multiple="true" :close-on-select="false" :clear-on-select="false" :preserve-search="true" placeholder="Pick some" label="name" track-by="name" :preselect-first="true">
            <template  slot-scope="{ values }"><span class="multiselect__single" >{{ values.length }} city selected</span></template>
          </multiselect>
           </div>
           <div class=" mt-2 text-left w-32">
             <b-button variant="primary" @click="filter" >Filter</b-button>
           </div>
       </form>
    </b-col>

    <b-col m="6" md="5" lg="8" class="mt-4">
      <b-list-group v-if="Array.isArray(filteredData)">
         <b-list-group-item v-show="filteredData"  v-for="(item,index) in filteredData" :key="index">
           Name :{{item.name}} |
           Age :{{item.age}} |
           City :{{item.city}}
        </b-list-group-item>
      </b-list-group>
    </b-col>
  </b-row>
</template>
<script>
  import Multiselect from 'vue-multiselect'
  import Vue from 'vue';
  Vue.component('multiselect', Multiselect)

export default {
  components: { Multiselect },
  mounted: function() {
      
        this.filterArray(this.dataSet,this.$route.query)
         
    },
  data: () => ({
    showAlert: true,
     initialData : {
          name: null,
          age: null,
          city: null,
       },
      tempData : [],
      filteredData : [],
      options: [
        { name: 'Ankara'},
        { name: 'Erzurum' },
        { name: 'İstanbul'},
        { name: 'Bursa' },
      ],
      dataSet : [
        {"name" :"Ahmet", "age":25, "city":"Ankara"},
        {"name" :"Mehmet", "age":22, "city":"Erzurum"},
        {"name" :"Ayşe", "age":23, "city":"İstanbul"},
        {"name" :"Fatma", "age":25, "city":"İstanbul"},
        {"name" :"Muhittin", "age":22, "city":"Ankara"},
        {"name" :"Ayça", "age":26, "city":"Bursa"}
      ]
  }),
  methods : {
  filterArray(array, filters) {
          this.filteredData = [];
          if(Object.keys(filters).length == 0 ){
            return false
          }

        
        this.initialData.name = filters.name?? null
        this.initialData.age =  filters.age?? null
        this.initialData.city = filters.city ? this.citySplitter(filters.city) : null
        
       return this.dataSet.filter((item) => {
          if(filters.age !== ''){
            if(item.age == filters.age && filters.name == '' ){
                this.filteredData.push(item)
            }else if (item.age == filters.age && filters.name == item.name){
                this.filteredData.push(item)
            }
          }else{
            return true
          }
        }).filter(x => {
          if(filters.name !== ''){
                if(x.name == filters.name && filters.age == '' ){
                this.filteredData.push(x)
              }else if (x.age == filters.age && filters.name == x.name){
                  this.filteredData.push(x)
              }
          }else{
            return true
          } 
        }).filter(y => {
             if(filters.city !== ''){
               if(y.city == filters.city){
                this.filteredData.push(y)
              }else{
                return true
              }
          }else{
            return true
          } 
        })


      },
      filter(){
            var city = '';
            if(Array.isArray(this.initialData.city) &&  this.initialData.city.length > 0){
               this.initialData.city.forEach(element => {
                city += element.name + '|'
            });
               city = city.slice(0, -1)
            }
           var searchObject = { 'name' : this.initialData.name, 'age':this.initialData.age, 'city' : city}
           this.filterArray(this.dataSet,searchObject)
           this.$router.push({path: 'search', query: searchObject}).catch(err => {console.log('Same Location : ',err)})
          
      },
      citySplitter (str) {
         let tempCities = [];
         let cityArray = str.split("|")
         cityArray.forEach(element => {
               tempCities.push({'name':element})
            });
         return tempCities

      },
 
      
  }
};
</script>

<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>