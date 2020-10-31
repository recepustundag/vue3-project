<template>
  <div class="bg-teal-200 min-h-screen py-12">
    <div class="max-w-md mx-auto bg-calculator rounded px-10 pt-8 pb-16">
      <div class="text-right process h-5">{{ transactions }}</div>
      <div class="text-right text-white text-3xl py-3 mb-8 h-12">{{ result }}</div>
      <div class="grid grid-cols-3 gap-1">
        <div class="grid grid-cols-3 gap-1 col-span-2">
          <div
            v-for="item in 9"
            :key="item"
            v-text="item"
            @click="press(item)"
            class="button-bg flex items-center justify-center h-16 text-white text-2xl rounded font-light cursor-pointer"></div>
        </div>
        <div class="grid grid-row-1 gap-1 col-span-1">
          <div @click="press('*')" class="button-bg flex items-center justify-center h-12 text-white text-xl rounded font-light cursor-pointer"> x </div>
          <div @click="press('/')" class="button-bg flex items-center justify-center h-12 text-white text-xl rounded font-light cursor-pointer"> / </div>
          <div @click="press('-')" class="button-bg flex items-center justify-center h-12 text-white text-xl rounded font-light cursor-pointer"> - </div>
          <div @click="press('+')" class="button-bg flex items-center justify-center h-12 text-white text-xl rounded font-light cursor-pointer"> + </div>
        </div>
      </div>
      <div class="grid grid-cols-3 gap-1 mt-1">
        <div @click="press('c')" class="button-bg flex items-center justify-center h-16 text-red-500 text-4xl rounded font-normal cursor-pointer"> c </div>
        <div @click="press(0)" class="button-bg flex items-center justify-center h-16 text-white text-2xl rounded font-light cursor-pointer"> 0 </div>
        <div @click="press('=')" class="button-bg flex items-center justify-center h-16 text-white text-2xl rounded font-light cursor-pointer"> = </div>
      </div>
    </div>
  </div>
</template>

<script>
import { onMounted, ref } from 'vue'
export default {
  name: "Calculator",
  setup(){
    const operator = ['+', '-', '*', '/'];
    const prevNum = ref("");
    const currentNum = ref("");
    const selectedOperation = ref("");
    const result = ref();
    const transactions = ref('');
    const pressNumber = ref(true);

    onMounted( () => {
      window.addEventListener("keypress", function(e) {
        let code = parseInt(e.key);
        if( Number.isNaN(code) ) {
          press(e.key);
        }else{
          press(code);
        }

      });
    });

    function press(val){
      if( val === 'c' ) clear();

      else if( val === '=' ) calculate();

      else if( operator.includes(val) ) applyOperation(val);

      else if( Number.isInteger(val) ) appendNumber(val);
      
    }

    function appendNumber(val) {
      pressNumber.value = true;
      currentNum.value = currentNum.value + val.toString();
      
      if( prevNum.value != '') {
        calculate();
      }

      transactions.value = transactions.value + val;
      
    }

    function applyOperation(val) {
      
      if( val !== 'c' && val !== '='){
        if( pressNumber.value ) transactions.value = transactions.value + val;
      }

      pressNumber.value = false;
      selectedOperation.value = val;
      typeof result.value !== 'undefined' && result.value != '' ? prevNum.value = result.value.toString() : prevNum.value = currentNum.value;
      currentNum.value = '';
    }

    function calculate() {
      
      if( typeof result.value === 'undefined' ) result.value = 0;
      
      if( selectedOperation.value === '*' ) multiplication();

      else if( selectedOperation.value === '+' ) sum();

      else if( selectedOperation.value === '-') subtract();

      else if( selectedOperation.value === '/') divide();
      
    }

    function multiplication() {
      result.value = parseInt(prevNum.value) * parseInt(currentNum.value);
    }

    function sum() {
      result.value = parseInt(prevNum.value) + parseInt(currentNum.value);
    }

    function subtract() {
      result.value = parseInt(prevNum.value) - parseInt(currentNum.value);
    }

    function divide() {
      result.value = parseInt(prevNum.value) / parseInt(currentNum.value);
    }
    
    function clear() {
      prevNum.value = '';
      currentNum.value = '';
      selectedOperation.value = '';
      result.value = '';
      transactions.value = '';
    }


    return { press, transactions, result}
  }
};
</script>

<style scoped>
  .bg-calculator{
    background: #3a4655;
  }
  .button-bg{
    background: #374352;
  }
  .button-bg:active{
    background: #313c4a;
  }
  .process{
    color: #647989
  }
</style>