<template>
   <section>
    <h1>Chart</h1>
    
    <Bar
    :chart-options="chartOptions"
    :chart-data="chartData"
    :chart-id="chartId"
    :dataset-id-key="datasetIdKey"
    :plugins="plugins"
    :css-classes="cssClasses"
    :styles="myStyles"
    :width="width"
    :height="height"
  />
   </section>
  </template>
  
  <script>
  import { Bar } from 'vue-chartjs'
  import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'
  import axios from "axios";
  ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)
  
  export default {
    name: 'BarChart',
    components: { Bar },
   
    data() {
      return {
        
     
        chartData: {
            labels: [ "one","two","three","Four" ],
            datasets: [

 
]
        },
        chartOptions: {
          responsive: true
        }
      }
    },
    computed:{
      myStyles () {
      return {

        maxWidth:"450px",
        
        // backgroundColor:'blue'
      }
    }
    },
  
    methods: {
        fetchData(){
    

            const options = {
            method: 'GET',
            url: 'https://covid-193.p.rapidapi.com/statistics',
            headers: {
                'X-RapidAPI-Key': 'd2dcdef782msh52b119a3a9d64bap18f3f4jsn2f543e572c48',
                'X-RapidAPI-Host': 'covid-193.p.rapidapi.com'
            }
};



            const self = this 
          
          
            axios.request(options).then(function (response) {
            
              let loopdata=response.data.response
              let empty=[]
              let arr_cases=[]
              let arr_tests=[]
              loopdata.forEach(element => {                        
                let el=element.time 
                 self.chartData.labels.push(el.substring(11,19))
              
                const deaths =element.deaths.total
                const new_cases=element.cases.new
                const new_tests=element.tests.total
                // console.log(new_tests);
                empty.push(deaths)
                arr_cases.push(new_cases) 
                arr_tests.push(new_tests)
                  ;
                //  console.log(deaths);
                 
              });
              let obj={
                data:empty,
                backgroundColor: "blue",
                label:'Total number of deaths',
                barPercentage: 25.5
              }
              let ob2=  {
                barPercentage: 35.5,
                  label: 'New corona cases',
                  data: arr_cases,
                  backgroundColor: 'red',
                  borderColor:'black',
                  borderWidth: 1
                }
                let ob3=  {
                barPercentage: 30.5,
                  label: 'Tests done',
                  data: arr_tests,
                  backgroundColor: 'green',
                  borderColor:'purple',
                  borderWidth: 1
                }
              let data_sets= self.chartData.datasets
              data_sets.push(obj,ob2,ob3)
              console.log()
            }).catch(function (error) {
                console.error(error);
            });
        }
    },
    mounted() {
     this.fetchData()
      
      
    },

  }
  </script>