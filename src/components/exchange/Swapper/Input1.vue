<template>
  <div class="relative flex w-full items-center st5 focus-within:text-oswapGreen-dark dark:focus-within:text-oswapGreen dark:text-oswapGreen text-gray-500">
    <input type="text" :value="getInput()" @input="lazyCaller" :class="this.invalidInput ? 'ring-red-400 focus:ring-red-400 ring-1 focus:ring-1 focus:outline-none ' + this.rounded : 'ring-black ring-opacity-10 focus:ring-oswapGreen ring-1 focus:outline-none ' + this.rounded" class="flex w-full z-20 st5 bg-gray-100 py-2 items-center pl-3 dark:bg-oswapDark-gray dark:placeholder-gray-600 placeholder-gray-300" :placeholder="placeholder">
    <slot />
    <!-- display errors -->
    <transition name="invalid-input-body">
      <div v-if="this.invalidInput" class="flex z-10 w-full absolute py-2 my-2 top-0 left-0 bg-red-400 rounded-xl shadow-lg" :class="errorTop">
        <transition-group tag="div" name="invalid-input-list" class="relative flex flex-col space-y-2">
          <div v-for="(error, index) in this.errors" :key="index" class="text-xs text-gray-50 mx-3 flex">
            {{error}}
          </div>
        </transition-group>
      </div>
    </transition>
  </div>
</template>

<script>
  import openswap from "@/shared/openswap.js";
  import { mapActions, mapGetters } from 'vuex';

  export default {
    mixins: [openswap],

    name: 'Input1',
    props: {
      errors: Object,
      rounded: String,
      placeholder: String,
      errorTop: String
    },
    data(){
        return{
          inputValue: 0
        }
      }, 
    computed: {
      ...mapGetters('exchange/swapper', ['getInputAmount', 'getSlippageRate', 'getPriceImpact', 'getInputAmount' ,'getLastSelected']),
           
      invalidInput() {
        if (Object.keys(this.errors).length > 0) {
          return true
        } else { 
          return false
        }
      }
    },
    methods: {
      ...mapGetters('exchange', ['getToken']),
      ...mapActions('exchange/swapper/buttons', ['setBtnState']),
      ...mapActions('exchange/swapper', [
        'setInputAmount', 
        'setPriceImpact', 
        'setThePath',
        'setWarning', 
        'deleteWarning',
        'setLastSelected'
      ]),

        lazyCaller(event, time = 200) {
      clearTimeout(this.timeout);
      this.timeout = setTimeout(() => {
        this.setLastSelected(1) 
        this.inputValue = event.target.value
        this.$emit('input1', event.target.value)
      }, time)
    },

  
      getInput: function(){
        if(this.getLastSelected == 0 || this.getLastSelected == 3){
          return this.getInputAmount(1)
        }else{
          return this.inputValue
        }
      }
    }
  }
</script>