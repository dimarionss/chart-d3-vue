<template>
  <v-card class="pa-10" rounded="xl" elevation="8">
    <v-row>
      <v-col cols="12">
        <v-row>
          <v-col cols="12" md="6">
            <h2 class="text-grey text-h4">Demand mathing on business liness</h2>
          </v-col>
          <v-col cols="12" md="6" class="d-flex justify-end">
            <v-btn-toggle v-model="toggle" color="#14248A" base-color="grey-lighten-4" mandatory rounded="xl"
              class="toggle bg-grey-lighten-4" density="compact">
              <v-btn value="quarter" rounded="xl" class="text-capitalize text-body-1" size="large">Quarter</v-btn>
              <v-btn value="month" rounded="xl" class="text-capitalize text-body-1" size="large">Month</v-btn>
            </v-btn-toggle>
          </v-col>
        </v-row>
      </v-col>
      <v-col cols="12">
        <HeatMap :dataChart="dataChart" />
      </v-col>
    </v-row>
  </v-card>
</template>

<script lang="ts" setup>
import { storeToRefs } from 'pinia'
import dayjs from 'dayjs';
import { computed, onBeforeMount, ref, watch } from 'vue'
import HeatMap from "../components/Charts/HeatMap.vue";
import { useRouter } from 'vue-router'

const countData = ref(25)
const toggle = ref('quarter')
const dataChart = ref([])

const generateQuarterData = () => {
  const quarters = ['Quarter 1', 'Quarter 2', 'Quarter 3', 'Quarter 4']
  const data = []
  for (let i = 0; i < countData.value; i++) {
    for (let j = 0; j < quarters.length; j++) {
      data.push({
        row: `Business Value ${i + 1}`,
        col: quarters[j],
        value: Math.floor(Math.random() * 100) + 1
      })
    }
  }
  return data
}

const generateMonthData = () => {
  const startMonth = dayjs().month()
  const months = Array.from({ length: 12 }, (_, i) => dayjs().month((startMonth + i) % 12).format('MMMM'))
  const data = []
  for (let i = 0; i < countData.value; i++) {
    for (let j = 0; j < months.length; j++) {
      data.push({
        row: `Business Value ${i + 1}`,
        col: months[j],
        value: Math.floor(Math.random() * 100) + 1
      })
    }
  }
  return data
}

const updateChartData = () => {
  if (toggle.value === 'quarter') {
    dataChart.value = generateQuarterData()
  } else {
    dataChart.value = generateMonthData()
  }
}

watch(toggle, () => {
  updateChartData()
  console.log(dataChart.value)
}, { immediate: true })

onBeforeMount(() => {
  updateChartData()
})

</script>
<style lang="scss" scoped></style>
