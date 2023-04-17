<template>
  <q-page class="row justify-center">
    <div class="column col-xs-11 col-sm-8 col-md-6 col-lg-4">
      <div class="row q-pt-xl">
        <div class="col col-5"><q-input class="without_number"
            v-model="numberOfDoughBalls.value" label="Number of Dough Balls"
            type="number" />

        </div>
        <div class="col col-1  q-pr-sm">
          <q-btn square dense flat color="secondary" icon="expand_less"
            @click="incrementNumberOfDoughBalls" />
          <q-btn square dense flat color="secondary" icon="expand_more"
            @click="decrementNumberOfDoughBalls" />
        </div>
        <div class="col col-5 q-pl-sm"><q-input v-model="weightOfDoughBall.value"
            label="Weight of Dough Ball" type="number" min="0" max="500"
            step="5" /></div>
        <div class="col col-1">
          <q-btn square dense flat color="secondary" icon="expand_less"
            @click="incrementWeightOfDoughBall" />
          <q-btn square dense flat color="secondary" icon="expand_more"
            @click="decrementWeightOfDoughBall" />
        </div>


      </div>
      <div class="row q-pt-md">
        <div class="col col-5">
          <q-input v-model="hydration.value" label="hydration (%)" type="number"
            min="0" max="100" step="5" />
        </div>
        <div class="col col-1 q-pr-sm">
          <q-btn square dense flat color="secondary" icon="expand_less"
            @click="incrementHydration" />
          <q-btn square dense flat color="secondary" icon="expand_more"
            @click="decrementHydration" />
        </div>
        <div class="col col-5 q-pl-sm">
          <q-input v-model="salt.value" label="salt (%)" type="number" min="0"
            max="100" step="1" />
        </div>
        <div class="col col-1">
          <q-btn square dense flat color="secondary" icon="expand_less"
            @click="incrementSalt" />
          <q-btn square dense flat color="secondary" icon="expand_more"
            @click="decrementSalt" />
        </div>

      </div>
      <div class="row q-pt-md q-pr-xl">
        <div class="col col-12">
          <q-select v-model="selectedYeastType" :options="yeastTypes"
            label="Yeast type"></q-select>
        </div>
      </div>
      <div class="row q-pt-xl">

        <div class="row col-6">
          <div class="col col-12 text-h4 text-primary">
            {{ flour.value.toFixed(0) }}g
          </div>
          <div class="col col-12 text-h6">Flour</div>
        </div>

        <div class="row col-6">
          <div class="col col-12 text-h4 text-primary">
            {{ water.value.toFixed(0) }}g
          </div>
          <div class="col col-12 text-h6">Water</div>
        </div>
      </div>

      <div class="row q-pt-md">
        <div class="row col-6">
          <div class="col col-12 text-h4 text-primary">
            {{ ((flour.value + water.value) / (1 +
              (hydration.value / 100)) * (salt.value / 100)).toFixed(0) }}g
          </div>
          <div class="col col-12 text-h6">Salt</div>

        </div>
        <div class="row col-6">
          <div class="col col-12 text-h4 text-primary">
            {{ selectedYeastType.value === 'Dry' ? (((flour.value + water.value) /
              (1
                + (hydration.value / 100))) / 500).toFixed(1) : (((flour.value +
                  water.value) /
                  (1
                    + (hydration.value / 100))) / 200).toFixed(1) }}g
          </div>
          <div class="col col-12 text-h6">Yeast</div>
        </div>
      </div>

    </div>
  </q-page>
</template>

<script setup lang="ts">



import { ref, watch, onMounted } from 'vue';
import { NumberOfDoughBalls, WeightOfDoughBall, Water, Flour, Hydration, Salt, Yeast } from 'components/models';

// fresh yeast = moka/200
// dry yeast = moka/500

const numberOfDoughBalls = ref<NumberOfDoughBalls>({
  value: 4
});

const weightOfDoughBall = ref<WeightOfDoughBall>({
  value: 250
});

const hydration = ref<Hydration>({
  value: 70
});

const flour = ref<Flour>({
  value: 0
});

const water = ref<Water>({
  value: 0
});

const salt = ref<Salt>({
  value: 3
});

const yeast = ref<Yeast>({
  value: 0
});

const selectedYeastType = ref({ label: 'Dry yeast', value: 'Dry' });

const yeastTypes = ref([
  {
    label: 'Dry yeast',
    value: 'Dry'
  },
  {
    label: 'Fresh yeast',
    value: 'Fresh'
  }
])


onMounted(() => {
  numberOfDoughBalls.value.value = 4
})

function incrementNumberOfDoughBalls() {
  numberOfDoughBalls.value.value += 1
  //return NumberOfDoughBalls.value.value
}

function decrementNumberOfDoughBalls() {
  numberOfDoughBalls.value.value -= 1
  //return NumberOfDoughBalls.value.value
}

function incrementWeightOfDoughBall() {
  weightOfDoughBall.value.value += 5
  //return NumberOfDoughBalls.value.value
}

function decrementWeightOfDoughBall() {
  weightOfDoughBall.value.value -= 5
  //return NumberOfDoughBalls.value.value
}

function incrementHydration() {
  hydration.value.value += 1
  //return NumberOfDoughBalls.value.value
}

function decrementHydration() {
  hydration.value.value -= 1
  //return NumberOfDoughBalls.value.value
}

function incrementSalt() {
  salt.value.value += 1
  //return NumberOfDoughBalls.value.value
}

function decrementSalt() {
  salt.value.value -= 1
  //return NumberOfDoughBalls.value.value
}

watch([numberOfDoughBalls.value, weightOfDoughBall.value, hydration.value, salt.value, selectedYeastType.value], () => {
  const total = numberOfDoughBalls.value.value * weightOfDoughBall.value.value
  const saltInternal = total / (1 + (hydration.value.value / 100)) * (salt.value.value / 100)
  console.log(saltInternal)
  flour.value.value = total / (1 + (hydration.value.value / 100)) - total / (1 + (hydration.value.value / 100)) * (salt.value.value / 100)
  water.value.value = total - flour.value.value
}, { immediate: true });


</script>

<style lang="scss">
input[type="number"]::-webkit-outer-spin-button,
input[type="number"]::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>