<script setup>
import { ref, computed } from 'vue'

const inputText = ref('')
const formattedOutput = ref({})

const categories = {
  設備: [
    { fullName: 'C600 主機(1台), 白色, 台灣版', shortName: 'C600' },
    { fullName: 'C360 主機(1台), 白色, 台灣版', shortName: 'C360' },
    { fullName: 'C260 主機(1台), 白色, 台灣版', shortName: 'C260' },
    { fullName: 'C200 主機(1台), 白色, 台灣版', shortName: 'C200' },
    { fullName: 'A1 主機(1台), 白色, 台灣版', shortName: 'A1' },
    { fullName: 'L10主機(1台), 白色, 台灣版', shortName: 'L10' },
    { fullName: 'L1主機(1台), 黑色, 台灣版', shortName: 'L1' },
    { fullName: 'M1 主機(1台), 白色, 台灣版', shortName: 'M1' },
    { fullName: 'Smini 主機(1台), 銀色, 台灣版', shortName: 'Smini' },
    { fullName: 'Snano 主機(1台), 黑色, 台灣版', shortName: 'Snano黑' },
    { fullName: 'Snano 主機(1台), 藍色, 台灣版', shortName: 'Snano藍' },
    { fullName: 'Snano 主機(1台), 綠色, 台灣版', shortName: 'Snano綠' },
    { fullName: 'Snano 主機(1台), 紅色, 台灣版', shortName: 'Snano紅' },
    { fullName: 'S1主機(1台), 玫瑰金, 台灣版', shortName: 'S1玫瑰金' },
    { fullName: 'S1主機(1台), 白色, 台灣版', shortName: 'S1' }
  ],
  濾網: [
    { fullName: 'C600 前置濾網 Breathe Carbon (1盒), 台灣版', shortName: 'C600Carbon' },
    { fullName: 'C600 前置濾網 Breathe Bio (1盒), 台灣版', shortName: 'C600Bio' },
    { fullName: 'C600 主濾網 Breathe Combo (1盒), 台灣版', shortName: 'C600Combo' },
    { fullName: 'C600 主濾網 Breathe Odors (1盒), 台灣版', shortName: 'C600Odors' },
    { fullName: 'C360 主濾網 Breathe Odors (1盒), 台灣版', shortName: 'C360Odors' },
    { fullName: 'C360 前置濾網 Breathe Carbon (1盒), 台灣版', shortName: 'C360Carbon' },
    { fullName: 'C260 主濾網 Breathe Odors (1盒), 台灣版', shortName: 'C260Odors' },
    { fullName: 'C260 前置濾網 Breathe Bio (1盒), 台灣版', shortName: 'C260Bio' },
    { fullName: 'C200 主濾網 Breathe Combo (1盒), 台灣版', shortName: 'C200Combo' },
    { fullName: 'C200 主濾網 Breathe Odors (1盒), 台灣版', shortName: 'C200Odors' },
    { fullName: 'C200 前置濾網 Breathe Bio (1盒), 台灣版', shortName: 'C200Bio' },
    { fullName: 'C200 前置濾網 Breathe Carbon (1盒), 台灣版', shortName: 'C200Carbon' },
    { fullName: 'M1 濾網(2入組), 台灣版', shortName: 'M1濾網' }
  ],
  配件: [
    { fullName: 'M1 配件車袋(1個), 台灣版', shortName: 'M1 車袋' },
    { fullName: 'M1 電源供應器(1個), 台灣版', shortName: 'M1 電源' },
    { fullName: 'Smini 配件電池盒(1個), 台灣版', shortName: 'Smini 電池盒' },
    { fullName: 'Smini 配件LG 18650電池(2入組), 台灣版', shortName: 'Smini 電池' },
    { fullName: 'Smini 配件皮帶(1個), 台灣版', shortName: 'Smini 皮帶' },
    { fullName: 'Smini 配件電源供應器(1個), 歐洲版', shortName: 'Smini 電源(歐洲)' },
    { fullName: 'Smini 配件電源供應器(1個), 台灣版', shortName: 'Smini 電源(台灣)' },
    { fullName: 'Smini 配件桌架(1個), 台灣版', shortName: 'Smini 桌架' },
    { fullName: 'Smini 配件吸頂/壁掛(1個), 台灣版', shortName: 'Smini 壁掛' },
    { fullName: 'Snano 配件擴香片(10入組), 台灣版', shortName: 'Snano 擴香片10入' },
    { fullName: 'Snano 配件支架(1個), 台灣版', shortName: 'Snano 支架' },
    { fullName: 'Snano 配件精油(1瓶)健康呼吸, 台灣版', shortName: 'Snano精油-健康呼吸' },
    { fullName: 'Snano 配件精油(1瓶)樂活無鬱, 台灣版', shortName: 'Snano精油-樂活無鬱' },
    { fullName: 'Snano 配件精油(1瓶)安神好眠, 台灣版', shortName: 'Snano精油-安神好眠' },
    { fullName: 'Snano 配件精油(1瓶)清新健腦, 台灣版', shortName: 'Snano精油-清新健腦' }
  ]
}

function parseData(text) {
  const lines = text.trim().split('\n')
  const data = {}

  for (const line of lines.slice(1)) {
    const cols = line.split('\t')
    const name = cols[1]?.trim()
    if (!name) continue
    data[name] = {
      新品: cols[3]?.trim() || '',
      福利品: cols[4]?.trim() || '',
      良品: cols[5]?.trim() || '',
      不良品: cols[6]?.trim() || ''
    }
  }
  return data
}

function processData() {
  const parsed = parseData(inputText.value)
  const result = {}

  for (const [category, items] of Object.entries(categories)) {
    result[category] = items.map(({ fullName, shortName }) => ({
      name: shortName,
      ...(parsed[fullName] || { 新品: '', 福利品: '', 良品: '', 不良品: '' })
    }))
  }

  formattedOutput.value = result
}

function clearData() {
  inputText.value = ''
  formattedOutput.value = {}
}

const currentYear = new Date().getFullYear()
const years = Array.from({ length: 101 }, (_, i) => currentYear - i)
const months = Array.from({ length: 12 }, (_, i) => i + 1)

const selectedYear = ref('')
const selectedMonth = ref('')
const selectedDay = ref('')

const days = computed(() => {
  const year = Number(selectedYear.value)
  const month = Number(selectedMonth.value)

  if (!year || !month) return []

  const lastDay = new Date(year, month, 0).getDate()
  return Array.from({ length: lastDay }, (_, i) => i + 1)
})
</script>

<template>
  <div class="p-1">
    <div class="d-flex justify-content-center">
      <h1 class="text-xl font-bold mb-4">盤點資料整理工具</h1>
    </div>
    <div class=" gap-2 d-flex justify-content-center mb-3">
      <!-- 年 -->
      <select v-model="selectedYear">
        <option disabled value="">選擇年</option>
        <option v-for="year in years" :key="year" :value="year">{{ year }} 年</option>
      </select>

      <!-- 月 -->
      <select v-model="selectedMonth">
        <option disabled value="">選擇月</option>
        <option v-for="month in months" :key="month" :value="month">{{ month }} 月</option>
      </select>

      <!-- 日 -->
      <select v-model="selectedDay">
        <option disabled value="">選擇日</option>
        <option v-for="day in days" :key="day" :value="day">{{ day }} 日</option>
      </select>
      <button class="bg-blue-600  px-4 py-2 select">查詢</button>
    </div>

    <div class="d-flex justify-content-center">
      <textarea v-model="inputText" rows="10" class="w-75 border p-2 rounded mb-4" placeholder="貼上原始資料（含料號、品名敘述、合計數量、倉別）"></textarea>
    </div>
    <div class="d-flex justify-content-center">
      <button @click="processData" class="bg-blue-600  px-4 py-2 rounded me-2">整理資料</button>
      <button @click="clearData" class="bg-gray-400  px-4 py-2 rounded">清除</button>
    </div>
    <div v-if="Object.keys(formattedOutput).length" class="mt-6 overflow-auto ">
      <div class="d-flex justify-content-center">
        <h2 class="text-lg font-semibold mb-2 mt-5">整理後資料</h2>
      </div>
      <div class="d-flex justify-content-center mb-5">
        <table class="table-auto w-full border-collapse text-sm">
          <thead>
            <tr class="bg-gray-200">
              <th class="border px-4 py-2 text-left align-middle" style="font-size: 8px;">品名敘述</th>
              <th class="border px-4 py-2 text-right align-middle" style="font-size: 8px;">新品</th>
              <th class="border px-4 py-2 text-right align-middle" style="font-size: 8px;">福利品</th>
              <th class="border px-4 py-2 text-right align-middle" style="font-size: 8px;">良品</th>
              <th class="border px-4 py-2 text-right align-middle" style="font-size: 8px;">不良品</th>
            </tr>
          </thead>
          <tbody>
            <template v-for="(rows, category) in formattedOutput" :key="category">
              <tr v-for="row in rows" :key="row.name">
                <td class="border px-4 py-2 text-left align-middle" style="font-size: 8px;">{{ row.name }}</td>
                <td class="border px-4 py-2 text-right align-middle" style="font-size: 8px;">{{ row.新品 || '' }}</td>
                <td class="border px-4 py-2 text-right align-middle" style="font-size: 8px;">{{ row.福利品 || '' }}</td>
                <td class="border px-4 py-2 text-right align-middle" style="font-size: 8px;">{{ row.良品 || '' }}</td>
                <td class="border px-4 py-2 text-right align-middle" style="font-size: 8px;">{{ row.不良品 || '' }}</td>
              </tr>
            </template>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<style scoped>
body {
  font-family: sans-serif;
}
th, td {
  white-space: nowrap;
  vertical-align: middle;
}
table th, table td {
  font-size: 8px;
}
.select{
  border-radius: 30px;
}
</style>