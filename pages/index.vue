<template>
  <div class="relative flex items-top justify-center min-h-screen bg-gray-100 sm:items-center sm:pt-0 back bg" >

    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css" rel="stylesheet">

    <div class="max-w-4xl mx-auto sm:px-6 lg:px-8">
      
      <div class="mt-8 bg-white overflow-hidden shadow sm:rounded-lg p-6">
        <b-img src="/logo-bmi.png" ></b-img>
        <div class="row">
          <div class="col-8">
            <h2 class="text-2xl leading-7 font-semibold">
              {{lang.topic}}
            </h2>
          </div>
          <div class="col-4 ">
            <b-button block v-b-popover.hover.top = "'เปลี่ยนเป็นภาษาไทย'" variant="warning" v-on:click="changLanguageTh()" >TH</b-button>
            <b-button block v-b-popover.hover.top = "'switch to english'" style="float: right;" variant="success " v-on:click="changLanguageEng()">ENG</b-button>
          </div>
        </div>
        <p class="mt-3 text-gray-550">
              {{lang.detail}}
            </p>

      

        <div class="mt-4 pt-4 text-gray-800 border-t border-dashed">
          <b-form @submit.prevent="onSubmit()" @reset="onReset()">

        <b-form-group id="input-group-1" label-for="input-weight">
          <div class="row">
            <div class="col-5">
              <span>{{lang.dataWeight}}</span>
            </div>
            <div class="col-7">
              <b-form-input type="number" v-model="form.weight" id="input-weight" placeholder="0"/>
            </div>
          </div>
        </b-form-group>

          

        <b-form-group id="input-group-2" label-for="input-height">
          <div class="row">
            <div class="col-5">
              <span>{{lang.dataHeight}}</span>
            </div>
            <div class="col-7">
              <b-form-input type="number" v-model="form.height" id="input-height" placeholder="0"/>
            </div>
          </div>
          
        </b-form-group>

        <b-button v-b-popover.hover.right = lang.hover_text_ok type="submit" block variant="primary">{{lang.bCal}}</b-button>
        <b-button v-b-popover.hover.right = lang.hover_text_delete type="reset" block variant="danger">{{lang.bReset}}</b-button>
      </b-form>

      <b-modal v-model="showResult" id="modal-center" centered  title="BMI Result" ok-only>
        <p>BMI: {{response.bmi}}</p>
        <p>Interpretation: {{response.interpretation}}</p>
        <p>Weight: {{response.weight}}</p>
        <p>Height: {{response.height}}</p>

      </b-modal>

        </div>
       <div style="text-align:center;">
        <p class="mt-4 pt-4 text-gray-800 credit-font   ">
          <b-badge href="#" variant="secondary" style="font-size:15px;">{{lang.creadit_text}}</b-badge> <br>
          {{lang.credit}}
        </p>

      </div>
      </div>
      
       
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
 data() {
   return {
     form: {
       weight:0,
       height:0,
     },
     response:{
       weight:0,
       height:0,
       bmi:0,
       interpretation:'',
     },
     showResult:false,
     lang:{
       topic : 'Body Mass Index (BMI)',
       detail : 'This work is part of workshop1.',
       dataWeight : 'Your Weight(kg) :',
       dataHeight : 'Your Height(cm) :',
       bCal : 'Calculate',
       bReset : 'Delete Text',
       credit : 'Thanapon Visedsang , Student number is 6121600233',
       creadit_text: 'Create By',
       alert_text: 'Please check the information you entered. that is correct or not',
       hover_text_ok: 'Your sure ? ',
       hover_text_delete: 'Click for DELETE ? '
     }
   }
 },
 methods: {
   onSubmit(event) {
     if(this.form.weight < 1 || this.form.height < 1){
       alert(this.lang.alert_text);
     }else{
      let param = 'weight=' + this.form.weight + '&height=' + this.form.height;
      let apiUrl ='https://pirun.ku.ac.th/~faaspsu/edu/mobile/evaluatebmi.php?'+ param;
      axios.get(apiUrl).then(res =>{
       this.response.weight = res.data.bmidata.weight;
       this.response.height = res.data.bmidata.height;
       this.response.bmi = res.data.bmidata.bmi;
       this.response.interpretation = res.data.bmidata.interpretation;
       this.showResult=true;
       console.log(this.response);
     }) 
    }
     

   },
   onReset(){
   },
   changLanguageTh(){
     this.lang = {
       topic : 'ดัชนีมวลกาย (BMI)',
       detail : 'ผลงานนี้เป็นส่วนหนึ่งของรายวิชา workshop1',
       dataWeight : 'กรอกน้ำหนัก (kg) :',
       dataHeight : 'กรอกส่วนสูง (cm) :',
       bCal : 'คำนวณ BMI',
       bReset : 'ล้างข้อมูล',
       credit : 'นายธนพล วิเศษสังข์ รหัสนิสิต 6121600233',
       creadit_text: 'จัดทำโดย',
       alert_text: 'กรุณาตรวจสอบความถูกต้องของข้อมูล ค่าที่ใส่จะต้องไม่เป็น 0 หรือน้อยกว่า 1',
       hover_text_ok: 'คุณแน่ใจนะ ว่าจะคำนวณ ? ',
       hover_text_delete: 'คุณต้องการลบข้อมูลใช่มั้ย ? '
     }
   },
   changLanguageEng(){
     this.lang = {
       topic : 'Body Mass Index (BMI)',
       detail : 'This work is part of workshop1.',
       dataWeight : 'Your Weight(kg) :',
       dataHeight : 'Your Height(cm) :',
       bCal : 'Calculate',
       bReset : 'Delete Text',
       credit : 'Thanapon Visedsang , Student number is 6121600233',
       creadit_text: 'Create By',
       alert_text: 'Please check the information you entered. that is correct or not',
       hover_text_ok: 'Your sure ? ',
       hover_text_delete: 'Click for DELETE ? '
     }
   }

 },
}
</script>

<style>
.bg{
    height: 800px; 
    background-image: linear-gradient(to bottom right , #8470FF, #FFB6C1);
    background-repeat: no-repeat;
    
}
.credit-font{
  font-size: 25px;
}
</style>