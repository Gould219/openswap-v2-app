<template>
  <transition name="fall" appear disappear>
  <div class="flex flex-wrap xs:w-full lg:w-half">
    <div v-if="validator" class="flex min-w-0 w-full text-gray-400 dark:text-gray-300">
      <div class="flex w-full flex-wrap rounded-lg py-3 px-4 items-center dark:from-slightDark from-slightGray to-transparent border border-oswapGreen">
        <div class="flex w-full dark:bg-oswapDark-gray bg-slightGray rounded-lg mb-2">

          <div class="flex flex-1 flex-col">
                <div class="flex items-center">
                  <i class="las la-sitemap pl-2 text-lg dark:text-oswapGreen"></i>
                  <p class="lg-text-2xl md-text-sm pl-2">{{ validator.name }}</p>
                </div>
                <div class="flex flex-wrap flex-1 justify-evenly my-2 text-center">
                  <div class="flex flex-col text-gray-400 dark:text-gray-300">
                      <p class="text-xl md-text-sm justify-center font-extrabold">{{ prettify(validator.userDelegations) }}</p>
                      <p class="text-xs md-text-sm justify-center font-bold text-gray-500 dark:text-gray-400">Staked</p>
                    </div>
                    <div class="flex flex-col text-gray-600 dark:text-gray-200">
                      <p class="text-xl justify-center  md-text-sm font-extrabold">{{ getApr() }} %</p>
                      <p class="text-xs justify-center md-text-sm font-bold text-gray-500 dark:text-gray-400">APR</p>
                    </div>
                    <div class="flex  flex-col  text-gray-400 dark:text-gray-300">
                      <p class="text-xl justify-center md-text-sm font-extrabold">{{ prettify(validator.totalDelegated) }}</p>
                      <p class="text-xs justify-center md-text-sm font-bold text-gray-500 dark:text-gray-400">Total Staked</p>
                    </div>
                </div>
          </div>
        </div>

        <div class="grid overflow-hidden md:grid-cols-2 sm:grid-cols-1 gap-3.5 w-full">
          <div>
            <ValidatorChart :validator="validator" />
            <p class="text-oswapGreen my-1">Info: Harmony validators have an undelegation period of approx. 6 days. however, this is the safest form of earning interest as funds never leave your wallet.</p>
            <ul class="flex mb-0 list-none flex-wrap py-2 flex-row">
              <li class="w-full text-oswapGreen my-1">Rewards for this validators default to 100% OpenX.</li>
              <li class="w-full text-oswapGreen my-1">
               <a href="https://docs.openswap.one/harmony-validator/20-apy-goal" target="_blank">
                For guides and additional information regarding our validator, please visit our official docs. (Click here)
               </a>
              </li>
              <li class="w-full text-oswapGreen my-1">Thank you for securing Harmony.</li>
            </ul>
          </div>
        <div>
        <div class="flex flex-wrap">
          <StakingInfoTabs v-if="!this.isFifteen()" :validator="validator" @updateValData="updateValData" />

          <!-- Open Details Button -->
          <div class="flex justify-end w-full mt-2">
            <div @click="selectValidator()" class="flex items-center space-x-2 rounded-full group bg-gray-100 hover:bg-gray-200 dark:bg-oswapDark-gray dark:hover:bg-gray-900 pl-3 cursor-pointer">
              <p class="text-sm text-gray-500 dark:text-oswapBlue-light">close</p>
              <i class="las la-arrow-up text-lg p-2 text-gray-500 dark:text-oswapBlue-light rounded-full bg-gray-100 dark:bg-oswapDark-gray group-hover:bg-oswapGreen dark:group-hover:text-oswapDark-gray group-hover:text-gray-100 border-2 border-gray-200 dark:border-gray-700"></i>
            </div>
          </div>
        </div>
          </div>
      </div>
    
       
      </div>
    </div>
  </div>
</transition>
</template>

<script>
import openswap from "@/shared/openswap.js";
import ValidatorChart from "@/components/farm/Validators/ValidatorChart";
import StakingInfoTabs from "@/components/farm/Validators/StakingInfoTabs";
  import { mapGetters } from 'vuex';

import { ethers } from "ethers";

export default {
  name: "StakingInfo",
  mixins: [openswap],
  components: {
    ValidatorChart,
    StakingInfoTabs,
  },
  props: {
    validator: Object,
  },
  data() {
  },
  async mounted() {},
  methods: {
    getApr : function(){
        return (parseFloat(this.validator.apr) + this.validator.addedAPR).toFixed(2)
      },
        getApy : function(){
        let totalAPR = parseFloat(this.getApr())
        return (totalAPR * ((1+0.68/365)**365-1) + totalAPR).toFixed(2)
      },
    isFifteen: function() {
      if(this.validator.name.includes('15%')){
        return true
      } else {
      return false
    }
    },
    selectValidator: function () {
      this.$emit("selectValidator", null);
    },
     updateValData: function () {
      this.$emit("updateValData", null);
    },
    prettify: function(number){
        if(number != "NA"){
          return ethers.utils.commify(String(number))
        }
        else{
          return number
        }
        
      }
  },
};
</script>

<style lang="scss" scoped></style>
