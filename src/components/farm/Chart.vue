<template>
  <div  class="flex items-center">
    <div class="flex">
      <apexchart class="text-grey-400" v-if="this.series.data != []" type="bar" width="400" height="200" :options="chartOptions" :series="series"></apexchart>
    </div>

  </div>
</template>

<script>
  import VueApexCharts from "vue3-apexcharts";
  import { ethers } from 'ethers'; 
  import {mapGetters } from 'vuex';

  export default {
    name: 'Chart',
    components: {
      apexchart: VueApexCharts
    },
    props: {
    	amount: String,
    	rewardPerWeek: Number
    },
    mounted() {

        const data = this.getData()
        this.series.name = "hello"
        this.series.data = data.usd
        console.log(this.series)
        //console.log(amounts)
        this.chartOptions.labels = data.name
        this.totalPools = data.name.length
    },
    methods: {
      ...mapGetters("farm/farmData", ["getTokens"]),
      prettify: function(number){
        return  ethers.utils.commify(number)
      },
      getCategories: function(){
      	var tokens = this.getTokens()
        var data = {
        	usd: [],
        	name: []
        }
        var usdValue = []
        var weeks = []
        var x = 0
    

        while(x < 12 ){

        	usdValue.push('Month ' + (x + 1))
        	x++
        }
  
        return usdValue
      },
      getSeries: function(){
      	var tokens = this.getTokens()
        var data = {
        	usd: [],
        	name: []
        }
        var usdValue = []
        var weeks = []
        var x = 0
    

        while(x < 12 ){

        	usdValue.push(parseFloat((parseFloat(this.amount) + parseFloat(this.rewardPerWeek* 4 * x)).toFixed(2) ))
        	x++
        }
  
        return usdValue
      },
      getData: function(){
        var tokens = this.getTokens()
        var data = {
        	usd: [],
        	name: []
        }
        var usdValue = []
        var weeks = []
        var x = 0
    

        while(x < 12){

        	usdValue.push(parseFloat((parseFloat(this.amount) + parseFloat(this.rewardPerWeek * 4 * x)).toFixed(2) ))
        	//weeks.push("Month " + x)
       
        	x++
        }
       data.usd = usdValue
       data.name = weeks
       console.log(data)
        return data
      }
    },
    data() {
      return {
        totalPools: 0 ,
        series:[{ 
        	data: this.getSeries(),
        	name: 'USD Value '
        }],
        chartOptions: {
          chart: {
            type: 'bar',
             toolbar: {
              show: false
            }
          },
          xaxis: {
          	categories: [],//this.getCategories(),
          	labels:{
          		style :{
          			colors: [ 
            '#109dbb', '#109dbb', '#109dbb', '#109dbb',
            '#109dbb', '#109dbb', '#109dbb', '#109dbb',
            '#109dbb', '#109dbb', '#109dbb', '#109dbb',
            '#109dbb', '#109dbb', '#109dbb', '#109dbb',
            '#1bf2ba', '#18d5bb', '#13b4ba', '#109dbb']
          		}
          	}
          },
          yaxis: {
        
          	labels:{
          		style :{
          			colors: [ 
            '#109dbb', '#109dbb', '#109dbb', '#109dbb',
            '#109dbb', '#109dbb', '#109dbb', '#109dbb',
            '#109dbb', '#109dbb', '#109dbb', '#109dbb',
            '#109dbb', '#109dbb', '#109dbb', '#109dbb']
          		}
          	}
          },
          responsive: [{
            breakpoint: 540,
            options: {
              chart: {
                width: 315,
                height: 190.
              }
            }
          }],
          labels: this.getCategories(),
          plotOptions: {
         
          },
          colors: [
            '#1bf2ba', '#18d5bb', '#13b4ba', '#109dbb',
            '#1bf2ba', '#18d5bb', '#13b4ba', '#109dbb',
            '#1bf2ba', '#18d5bb', '#13b4ba', '#109dbb',
            '#1bf2ba', '#18d5bb', '#13b4ba', '#109dbb',
            '#1bf2ba', '#18d5bb', '#13b4ba', '#109dbb',
            '#1bf2ba', '#18d5bb', '#13b4ba', '#109dbb'
          ],
          dataLabels: {
            enabled: false,
            style: {
            	colors: ['#E5E7EB']
            }
          },
          legend: {
            show: false,
          },
          tooltip: {
          	enabled: true
          },
          stroke: {
            show: true,
            curve: 'smooth',
            lineCap: 'round',
            colors: 'rgba(49, 53, 71, 1)',
            width: 1,      
          },
          tooltip: {
            theme: false,
            custom: ({ series, seriesIndex, dataPointIndex, w }) => {
              return (
                '<div class="flex p-3 rounded-md bg-gray-100 dark:bg-slightDark text-xs text-gray-500 dark:text-gray-200 border-l border-oswapGreen">' + "<span>" + "$ " + (series[seriesIndex][dataPointIndex]) + "</span>" + "</div>"
              );
            },
          },
        },
      };
    },
  }
</script>