<template>

  <!-- init vue template -->
  <div>

    <div class="h-[50vh] bg-sky-600 bg-gradient-to-r from-cyan-500 to-sky-600 relative mx-auto">

      <h1 class="text-4xl font-Zen absolute top-0 left-2 text-white">Currency Converter</h1>

        <div class="py-5">

        <input
            v-model="inputValue"
            class="bg-transparent absolute text-6xl font-thin font-Zen max-w-[300px] top-[45%] lg:left-[33%] md:left-[15%] left-5 outline-none pb-2 max-h-[50px] text-white"
            type="number"
        />

        <v-select 
          v-model="inputCurrency" 
          class="style-chooser text-3xl font-thin font-Zen top-[25%] lg:right-[33%] md:right-[15%] right-5 text-white" 
          :options="currencyNames">
        </v-select>

        </div>

    </div>

    <div class="flex justify-center items-center">
    <button class="rounded-full shadow-lg bg-white absolute hover:animate-spin z-10" @click="switchCurrency()">
        <span class="p-3 material-symbols-outlined text-6xl text-sky-600 font-semibold">
          currency_exchange
        </span>
    </button>
    </div>

    <div class="h-[50vh] relative mx-auto">

        <div class="py-5">

          <input
              disabled
              :value="convertedValue"
              class="absolute bg-transparent text-6xl font-thin font-Zen max-w-[300px] top-[45%] lg:left-[33%] md:left-[15%] left-5 outline-none text-sky-600"
              type="number"
          />
        
          <v-select 
            v-model="outputCurrency"
            class="style-chooser text-3xl font-thin font-Zen top-[25%] lg:right-[33%] md:right-[15%] right-5 text-sky-600" 
            :options="currencyNames">
          </v-select>

          <p class="absolute bottom-2 left-2 text-sky-600">Last updated: {{ date }}</p>

        </div>
        
    </div>

    </div>
</template>

<script>

import Vue from 'vue'
import vSelect from 'vue-select'
import 'vue-select/dist/vue-select.css';

Vue.component('v-select', vSelect)

export default {
      data() {
    return {
      listItems: {},
      date: {},
      currencyNames: [],
      inputValue: 1,
      inputCurrency: 'EUR',
      outputCurrency: 'ZAR',
    };
  },
  methods: {
    async getData() {
      const res = await fetch("https://api.exchangerate.host/latest");
      const finalRes = await res.json();
      this.date = finalRes.date
      this.listItems = finalRes.rates;
      this.currencyNames = Object.keys(this.listItems);
    },
    switchCurrency() {
      const initialCurrency = this.inputCurrency
      const initialValue = this.listItems[this.inputCurrency]
      this.inputCurrency = this.outputCurrency
      this.listItems[this.inputCurrency] = this.listItems[this.outputCurrency]
      this.outputCurrency = initialCurrency
      this.listItems[this.outputCurrency] = initialValue
    },
  },
  computed: {
    convertedValue() {
      if (!this.inputValue || !this.inputCurrency) {
        return 0
      }
     return Math.round(this.inputValue / this.listItems[this.inputCurrency] * this.listItems[this.outputCurrency] * 100) / 100;
    },
  },
  mounted() {
    this.getData();
  },
}
</script>

<style>
.style-chooser .vs__search::placeholder,
.style-chooser .vs__dropdown-toggle,
.style-chooser .vs__dropdown-menu {
  background: transparent;
  border: none;
  color: #394066;
  text-transform: lowercase;
  font-variant: small-caps;
  height: 63px;
}
.v-select {
  position: absolute;
}
.style-chooser #vs1__listbox, .style-chooser #vs2__listbox {
  width: 100px;
  height: 158px;
}
.style-chooser {
  width: 100px;
}
.vs__dropdown-menu {
  min-width: 100px;
}
.vs__dropdown-option {
  text-align: center;
}
.style-chooser .vs__clear {
  display: none;
}
.vs__selected {
  margin-top: 6px;
}
.vs__actions {
  margin-top: -3px;
}
.style-chooser .vs__dropdown-toggle {
  color: #fff;
}
#vs1__combobox {
  color: #fff;
}
#vs1__combobox .vs__selected-options {
  color: #fff;
}
#vs1__combobox .vs--single .vs__selected {
  color: #fff;
}
#vs1__combobox .vs__selected-options span.vs__selected {
  color: #fff;
}
#vs1__combobox .vs__open-indicator {
  fill: rgba(255, 255, 255, 0.803);
}
#vs1__listbox {
  color: #fff;
}
#vs2__combobox {
  color: #0284c7;
}
#vs2__combobox .vs__selected-options {
  color: #0284c7;
}
#vs2__combobox .vs--single .vs__selected {
  color: #0284c7;
}
#vs2__combobox .vs__selected-options span.vs__selected {
  color: #0284c7;
}
#vs2__combobox .vs__open-indicator {
  fill: #0285c7d8;
}
#vs2__listbox {
  color: #0284c7;
}
.vs__dropdown-option--highlight {
  background: #4338ca;
}
</style>
