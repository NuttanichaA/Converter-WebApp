<script setup>
import { ref, computed, watch } from 'vue';
const agree = ref(false)
const favConverter = ref([])
const converterType = ref('length')
const unitL = ref('centimetre')
const unitR = ref('inch')
const input = ref(0)
const output = ref(0)
const feedback = ref('')
const showForm = ref(true)

const centTometre = (input) => {
  return input / 1000
}
const centToInch = (input) => {
  return input * 2.54
}
const inchToCent = (input) => {
  return input / 2.54
}
const inchToMetre = (input) => {
  return input / 2.54 / 1000
}
const metretoCent = (input) => {
  return input * 1000
}
const metreToInch = (input) => {
  return input * 1000 * 2.54
}

const celcToKelv = (input) => {
  return input + 237.15
}
const kelvTocelc = (input) => {
  return input - 237.15
}
const fahrToCelc = (input) => {
  return (input - 32) * 5 / 9
}
const celcToFah = (input) => {
  return (input * 9 / 5) + 32
}
const fahrToKelv = (input) => {
  return ((input - 32) * 5 / 9) + 273.15
}
const kelvToFahr = (input) => {
  return ((input - 273.15) * 9 / 5) + 32
}

const convLengthCheck = computed(() => {
  if (converterType.value === 'length') {
    unitL.value = 'centimetre'
    unitR.value = 'inch'
    return true;
  } else {
    return false;
  }
})

const convTempCheck = computed(() => {
  if (converterType.value === 'temperature') {
    unitL.value = 'celcius'
    unitR.value = 'kelvin'
    return true;
  } else {
    return false;
  }
})

watch([input, unitL, unitR], () => {
  if (unitL.value === unitR.value) {
    output.value = input.value
  } else {
    if (converterType.value === 'length') {
      if (unitL.value === 'centimetre') {
        if (unitR.value === 'inch') {
          output.value = centToInch(input.value)
        } else {
          output.value = centTometre(input.value)
        }
      } else if (unitL.value === 'metre') {
        if (unitR.value === 'inch') {
          output.value = metreToInch(input.value)
        } else {
          output.value = metretoCent(input.value)
        }
      } else {
        if (unitR.value === 'centimetre') {
          output.value = inchToCent(input.value)
        } else {
          output.value = inchToMetre(input.value)
        }
      }
    } else {
      if (unitL.value === 'celcius') {
        if (unitR.value === 'kelvin') {
          output.value = celcToKelv(input.value)
        } else {
          output.value = celcToFah(input.value)
        }
      } else if (unitL.value === 'kelvin') {
        if (unitR.value === 'celcius') {
          output.value = kelvTocelc(input.value)
        } else {
          output.value = kelvToFahr(input.value)
        }
      } else {
        if (unitR.value === 'celcius') {
          output.value = fahrToCelc(input.value)
        } else {
          output.value = fahrToKelv(input.value)
        }
      }
    }
  }
},
  { immediate: true })

</script>

<template>
  <div class="m-2">
    <h1 class="text-3xl text-emerald-600 p-2">
      Converter
    </h1>
    <div id="form" v-show="showForm">
      <div id="policyCheckbox" class="p-2">
        <input type="checkbox" v-model="agree" />
        I read and except converter application policy.
      </div>
      <div id="favConverter" class="p-2">
        Favorite converter:
        <input type="checkbox" v-model="favConverter" value="Unit Converter"> Unit Converter
        <input type="checkbox" v-model="favConverter" value="Conversion Calculator"> Conversion Calculator
      </div>
      <div>
        <div id="converterType" class="p-2">
          <input type="radio" v-model="converterType" value="length"> Length
          <input type="radio" v-model="converterType" value="temperature"> Temperature
        </div>
        <div id="converter" class="flex flex-row h-32 items-center p-2">
          <div class="flex flex-col w-80 ">
            <input type="text" class="h-24 border border-gray-300 rounded-t-lg text-2xl text-center"
              v-model.number.trim="input">
            <select v-model="unitL" class="bg-gray-300 rounded-b-lg p-1 h-8">
              <option v-show="convLengthCheck" value="centimetre">Centimetre</option>
              <option v-show="convLengthCheck" value="metre">Metre</option>
              <option v-show="convLengthCheck" value="inch">Inch</option>
              <option v-show="convTempCheck" value="celcius">Celcius</option>
              <option v-show="convTempCheck" value="kelvin">Kelvin</option>
              <option v-show="convTempCheck" value="fahrenheit">Fahrenheit</option>
            </select>
          </div>
          <p class="text-3xl p-4">=</p>
          <div class="flex flex-col w-80 ">
            <label class="h-24 border border-gray-300 rounded-t-lg text-2xl flex items-center justify-center">{{ output
              }}</label>
            <select v-model="unitR" class="bg-gray-300 rounded-b-lg p-1 h-8">
              <option v-show="convLengthCheck" value="centimetre">Centimetre</option>
              <option v-show="convLengthCheck" value="metre">Metre</option>
              <option v-show="convLengthCheck" value="inch">Inch</option>
              <option v-show="convTempCheck" value="celcius">Celcius</option>
              <option v-show="convTempCheck" value="kelvin">Kelvin</option>
              <option v-show="convTempCheck" value="fahrenheit">Fahrenheit</option>
            </select>
          </div>
        </div>
        <div id="feedback" class="p-2">
          <p>Give us your feedback:</p>
          <textarea v-model="feedback" class="border border-gray-500 w-173 h-20 max-h-30"></textarea>
        </div>
      </div>
      <button class="bg-emerald-600 rounded-md text-white px-4 py-1 m-2 mt-0.5 font-semibold cursor-pointer"
      @click="showForm = false">
        submit
      </button>
    </div>
    <div id="summary" class="w-173 p-2" v-show="!showForm">
      <div>
        you are <span v-if="!agree" class="text-red-700">not</span> agree to our converter application policy,
        your favorite converter is <span v-if="favConverter.length">{{ favConverter.toString() }}</span>,
        and your comment is {{ feedback }} <span v-if="!feedback">-</span>.
      </div>
      <button class="bg-violet-600 rounded-md text-white px-4 py-1  mt-2 font-semibold cursor-pointer"
      @click="showForm = true">
        confirm
      </button>
    </div>
  </div>
</template>

<style scoped></style>