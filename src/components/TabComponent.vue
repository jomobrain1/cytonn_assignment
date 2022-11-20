<template>

     <section class="container">
          <div class="flex">
               <caption>Corona Virus Cases</caption>
               <div>
                   <select name=""  v-model="input" >
                        <option value="" selected>search for country</option>
                        <option v-for="c in countries" :value="c">{{c}}</option>
                      </select>
                      <button
                       @click="fetchHistory()"
                       type="button">search</button>
          </div>
          </div>
         <section class="height">
          <table class="responsive-table">
              
              
               <thead>
                 <tr>
                   <th scope="col">Continent</th>
                   <th scope="col">Country</th>
                   <th scope="col">Day</th>
                   <th scope="col">Population</th>
                   <th scope="col">Total Tests</th>
                   <th scope="col">Total Death</th>
                   
                 </tr>
               </thead>
               
               <tbody>
                   
                   
                 
                 <tr v-for="item in items" :key="item.id">
            <th scope="row">{{item.continent}}</th>
            <td>
              <span 
              style="font-size:13px;color:#2898D7;font-weight:800"
               class="hide_small">country</span> : 
              {{item.country}}
            </td>
            <td>
              <span 
              style="font-size:13px;color:#2898D7;font-weight:800"
               class="hide_small">date</span> : 
              {{item.day}}
            </td>
            <td>
              <span 
              style="font-size:13px;color:#2898D7;font-weight:800"
               class="hide_small">population</span> : 
              {{item.population}}
            </td>
            <td>
              <span 
              style="font-size:13px;color:#2898D7;font-weight:800"
               class="hide_small">tests</span> : 
              {{item.tests.total}}
            </td>
            <td>
              <span 
              style="font-size:13px;color:#2898D7;font-weight:800"
               class="hide_small">deaths</span> : 
              {{item.deaths.total}}
            </td>
                    
                 </tr>
                
                 
               </tbody>
           </table>
         </section>
          <div class="container">
               <Preloader v-if="loading"></Preloader>
               <div v-if="null_length">
                    <h1>No Data for the selected country</h1>
              </div>
          </div>
       </section> 
            
</template>
        
        
        <script>
        import axios from 'axios';
        import Preloader from './Preloader.vue'
        export default{
          components:{Preloader},
        data() {
        return {
            input:"",
            search:"",
            items:[],
            loading:false,         
            
            countries:[],
            null_length:false
        }
    },
        methods: {
          fetchCountries(){
            
        let self=this
        const options = {
          method: 'GET',
          url: 'https://covid-193.p.rapidapi.com/countries',
          headers: {
          'X-RapidAPI-Key': 'cb97d0d47amsh7a6bc47f73c69bcp19e4ffjsn925bcc1fa354',
          'X-RapidAPI-Host': 'covid-193.p.rapidapi.com'
          }
          };

       

        axios.request(options).then(function (response) {
          // console.log(response.data.response,"countries");
           self.countries=response.data.response
        }).catch(function (error) {
          console.error(error);
        });
      },
          fetchHistory(){
               // console.log(this.input);
               let self=this
               if(this.input==""){
                    alert('please select a country')
               }
               this.loading=true
               const options = {
               method: 'GET',
               url: 'https://covid-193.p.rapidapi.com/history',
               params: {country: self.input, day: '2020-06-02'},
               headers: {
                    'X-RapidAPI-Key': 'd2dcdef782msh52b119a3a9d64bap18f3f4jsn2f543e572c48',
                    'X-RapidAPI-Host': 'covid-193.p.rapidapi.com'
               }
               };
          
               
               axios.request(options).then(function (response) {
                    self.loading=false
                    // console.log(response.data.response.length);
                    let length=response.data.response.length
                    if(length==0){
                         
                         self.null_length=!self.null_length
                    }else{
                        self.null_length=false
                    }
                    self.items=response.data.response
                    
                    
               }).catch(function (error) {
                    console.error(error);
               });
               
               }
        },
        mounted() {
          this.fetchCountries()

        },
        created() {
      let myself=this
      this.loading=true
      const options = {
        method: 'GET',
        url: 'https://covid-193.p.rapidapi.com/statistics',
        headers: {
          'X-RapidAPI-Key': 'd2dcdef782msh52b119a3a9d64bap18f3f4jsn2f543e572c48',
           'X-RapidAPI-Host': 'covid-193.p.rapidapi.com'
        }
};
      axios.request(options).then(function (response) {
        // console.log(response.data.response);
        myself.loading=false
        myself.items=response.data.response
      }).catch(function (error) {
        console.error(error);
      })
      // console.log(this.items);
    },
        }
        </script>
        
     <style scoped>
     
     .height{
          
          padding: 30px 20px;
          height: min-content;
          max-height: 500px;
          overflow-y: scroll;
      }
    
     </style>