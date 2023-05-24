
<template>

  <div class="row">
    <div class="col-md-2"></div>
    <div class="col-md-8">
      <div class="card">
        <div class="card-header">
          {{name}}
        </div>
        <div class="card-body p-2">

        <form>
          <div class="mb-3">
            <label class="form-label">Text :</label>
<!--            <input type="text" class="form-control" name="text" @input="inputHandler">-->
            <input type="text" class="form-control" name="text" v-model="text">
            <h6 class="m-4">{{correct}}</h6>
          </div>
          <button  @click.prevent="clickHandler($event)" class="btn btn-primary">SubmitSubmit</button>
        </form>
        </div>
      </div>
    </div>
    <div class="col-md-2"></div>
    <div class="col-md-2"></div>
    <div class="col-md-8">
      <highcharts :options="chartOptions"></highcharts>
<!--      <Chart/>-->
    </div>
    <div class="col-md-2"></div>
  </div>

</template>

<script>
import * as Vue from 'vue' // in Vue 3

import axios from 'axios'
import VueAxios from 'vue-axios'
import {Chart} from 'highcharts-vue'

export default {
  components: {
    highcharts: Chart
  },

  data(){
    return{
      name: "HomePage",
      text: "",
      correct: "",
      result:[],
      keys:[],
      values:[],
      min:0,
      max:0,
      chartOptions: {
        series: [{
          data:[] // sample data
        }]
      }

    }

  },

  methods:{
    clickHandler(e){


      // axios.get("https://fakestoreapi.com/products").then((response) => {
      //   console.log(response.data)
      // })
      axios.post("http://127.0.0.1:8000/api/v1/text",{text:this.text}).then((response) => {
        if (response.data.status==1){
          Swal.fire({
            title: 'success!',
            text: 'Your request was succeed ...',
            icon: 'success',
            confirmButtonText: 'Cool'
          })
          this.result=response.data.result
          this.keys=response.data.keys
          this.values=response.data.values;
          this.min=response.data.min;
          this.max=response.data.max;
          this.correct=response.data.correct;

          this.chartOptions={
            chart: {
              type: 'line',
            },
            title: {
              text: 'Word Cloud ...',
            },

            xAxis: {
              categories: new Array([...this.keys])[0],
              title: {
                text: null,
              },
            },
            yAxis: {
              min: 0,
              title: {
                text: 'Number Of each word ...',
                align: 'high',
              },
              labels: {
                overflow: 'justify',
              },
            },
            tooltip: {
              valueSuffix: ' millions',
            },
            plotOptions: {
              bar: {
                dataLabels: {
                  enabled: true,
                },
              },
            },

            credits: {
              enabled: false,
            },
            series: [

              {
                name: 'Words ...',
                data: new Array([...this.values])[0],
              },
            ],
          }
        }else {
          Swal.fire({
            title: 'Error!',
            text: 'Do you want to continue',
            icon: 'error',
            confirmButtonText: 'Cool'
          })
        }
      }) .catch(error => {
        Swal.fire({
          title: 'Error!',
          text: 'Do you want to continue',
          icon: 'error',
          confirmButtonText: 'Cool'
        })
      });

      console.log(e)
    },
    inputHandler(e){
      console.log(e.target.value)
      this.text=e.target.value

    }
  }
}

</script>

