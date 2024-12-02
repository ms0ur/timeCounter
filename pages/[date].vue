<script setup lang="ts">
const route = useRoute()
const finalDate = new Date(route.params.date || new Date(new Date().getFullYear() + 1, 4, 1))
const countdown = ref('')
const miliseconds = ref(0)
const seconds = ref(0)
const minutes = ref(0)
const hours = ref(0)
const days = ref(0)
const months = ref(0)
const years = ref(0)

const secondPrecent = computed(() => {
  return ((miliseconds.value % 1000) / 1000 ) * 100
})
const minutePrecent = computed(() => {
  return ((miliseconds.value % 60000) / 60000 ) * 100
})
const hourPrecent = computed(() => {
  return ((miliseconds.value % 3600000) / 3600000 ) * 100
})
const dayPrecent = computed(() => {
  return ((miliseconds.value % 86400000) / 86400000 ) * 100
})
const monthPrecent = computed(() => {
  return ((miliseconds.value % 2592000000) / 2592000000 ) * 100
})
const yearPrecent = computed(() => {
  return ((miliseconds.value % 31536000000) / 31536000000 ) * 100
})

const fillValues = () => {
  const now = new Date()
  let diff = finalDate.getTime() - now.getTime();
  if (diff < 0) {
    return;
  }

  years.value = Math.floor(diff / (1000 * 60 * 60 * 24 * 365));
  months.value = Math.floor(diff / (1000 * 60 * 60 * 24 * 30));
  days.value = Math.floor(diff / (1000 * 60 * 60 * 24));
  hours.value = Math.floor(diff / (1000 * 60 * 60));
  minutes.value = Math.floor(diff / (1000 * 60));
  seconds.value = Math.floor(diff / 1000)
  miliseconds.value = diff
}

const generateString = ( useY, useM, useD, useH, useMin, useS) => {
  const now = new Date()
  let diff = finalDate.getTime() - now.getTime();
  if (diff < 0) {
    return 'Time has passed';
  }

  const years = Math.floor(diff / (1000 * 60 * 60 * 24 * 365));
  diff -= years * (1000 * 60 * 60 * 24 * 365);
  const months = Math.floor(diff / (1000 * 60 * 60 * 24 * 30));
  diff -= months * (1000 * 60 * 60 * 24 * 30)
  const days = Math.floor(diff / (1000 * 60 * 60 * 24));
  diff -= days * (1000 * 60 * 60 * 24);
  const hours = Math.floor(diff / (1000 * 60 * 60));
  diff -= hours * (1000 * 60 * 60);
  const minutes = Math.floor(diff / (1000 * 60))
  diff -= minutes * (1000 * 60);
  const seconds = Math.floor(diff / 1000)

  let string = ''
  if (useY && years > 0) {
    string += `${years} year${years > 1 ? 's' : ''} `
  }
  if (useM && months > 0) {
    string += `${months} month${months > 1 ? 's' : ''} `
  }
  if (useD && days > 0) {
    string += `${days} day${days > 1 ? 's' : ''} `
  }
  if (useH && hours > 0) {
    string += `${hours} hour${hours > 1 ? 's' : ''} `
  }
  if (useMin && minutes > 0) {
    string += `${minutes} minute${minutes > 1 ? 's' : ''} `
  }
  if (useS && seconds > 0) {
    string += `${seconds} second${seconds > 1 ? 's' : ''}`
  } else if (diff < 1000) {
    string += 'Less than a second'
  }
  return string
}

onMounted(() => {
  countdown.value = generateString(true, true, true, true, true, true)

  setInterval(() => {
    countdown.value = generateString(true, true, true, true, true, true)
    fillValues()
  }, 1)
})


</script>

<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-900 text-white p-6">
    <div class="max-w-xl w-full space-y-6">
      <h1 class="text-4xl font-extrabold mb-2 animate-fade-in">
        Time until {{ finalDate.getDate() }} {{ finalDate.toLocaleString('default', { month: 'long' }) }} {{ finalDate.getFullYear() }}:
      </h1>
      <p class="text-2xl font-semibold mb-8 animate-pulse">{{ countdown }}</p>

      <div class="text-center mb-6">
        <p class="text-xl font-medium">OR:</p>
      </div>

      <div class="grid grid-cols-2 gap-6">
        <div class="bg-gray-800 overflow-clip rounded-lg shadow-lg transform hover:scale-105 transition duration-300">
          <div class="p-6">
            <p class="text-sm uppercase text-gray-400 mb-2">Milliseconds</p>
            <p class="text-3xl font-bold">{{ miliseconds }}</p>
          </div>

        </div>
        <div class="bg-gray-800  rounded-lg shadow-lg transform hover:scale-105 transition duration-300">
          <div class="p-6">
            <p class="text-sm uppercase text-gray-400 mb-2">Full Seconds</p>
            <p class="text-3xl font-bold">{{ seconds }}</p>
          </div>
          <div class="pcbar" :style="`width: ${secondPrecent}%`"></div>
        </div>
        <div class="bg-gray-800 rounded-lg shadow-lg transform hover:scale-105 transition duration-300">
          <div class="p-6">
            <p class="text-sm uppercase text-gray-400 mb-2">Full Minutes</p>
            <p class="text-3xl font-bold">{{ minutes }}</p>
          </div>
          <div class="pcbar" :style="`width: ${minutePrecent}%`"></div>
        </div>
        <div class="bg-gray-800 rounded-lg shadow-lg transform hover:scale-105 transition duration-300">
          <div class="p-6">
            <p class="text-sm uppercase text-gray-400 mb-2">Full Hours</p>
            <p class="text-3xl font-bold">{{ hours }}</p>
          </div>
          <div class="pcbar" :style="`width: ${hourPrecent}%`"></div>
        </div>
        <div class="bg-gray-800 rounded-lg shadow-lg transform hover:scale-105 transition duration-300">
          <div class="p-6">
            <p class="text-sm uppercase text-gray-400 mb-2">Full Days</p>
            <p class="text-3xl font-bold">{{ days }}</p>
          </div>
          <div class="pcbar" :style="`width: ${dayPrecent}%`"></div>
        </div>
        <div class="bg-gray-800 rounded-lg shadow-lg transform hover:scale-105 transition duration-300">
          <div class="p-6">
            <p class="text-sm uppercase text-gray-400 mb-2">Full Months</p>
            <p class="text-3xl font-bold">{{ months }}</p>
          </div>
          <div class="pcbar" :style="`width: ${monthPrecent}%`"></div>
        </div>
        <div class="bg-gray-800 rounded-lg shadow-lg transform hover:scale-105 transition duration-300 col-span-2">
          <div class="p-6">
            <p class="text-sm uppercase text-gray-400 mb-2"> Full Years</p>
            <p class="text-3xl font-bold">{{ years }}</p>
          </div>
          <div class="pcbar" :style="`width: ${yearPrecent}%`"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.pcbar {
  background-color: #ff006e;
  height: 10px;
  border-radius: 16px;
}
@keyframes fadeIn {
  from { opacity: 0; }
  to   { opacity: 1; }
}
.animate-fade-in {
  animation: fadeIn 2s ease-in-out;
}
</style>

