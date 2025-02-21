<template>
    <!-- TIMER -->
    <div class="fixed top-0 right-14 p-2">
        <p>Time: {{formattedTime}}</p>
    </div>
    <!-- Screen + background -->
    <div class="w-screen h-screen bg-gray-100">
        <div v-for="(popup, index) in popups" :key="popup.id">
            <Popup :x="popup.x"
            :y="popup.y"
            :width="popup.width"
            :height="popup.height"
            :color="popup.color"
            @close="closePopup(index)"
            />
        </div>
    </div>
</template>
<script setup lang="ts">
interface Popup {
    id: number
    x: number 
    y: number
    width: number
    height: number
    color: string
}
const popups = ref<Popup[]>([])
const screenX = 1920 // fetch dynamicly
const screenY = 1080

const time = ref(0)

function generatePopups() {
for (let i = 0; i < 100; i++) {
    const popup: Popup = {
        id: i,
        x: randIntInclusive(0, screenX),
        y: randIntInclusive(0, screenY),
        width: randIntInclusive(100, 500),
        height: randIntInclusive(100, 500),
        color: '#'+(Math.random()*0xFFFFFF<<0).toString(16)
    }
    popups.value.push(popup);
}
}


function randIntInclusive(min: number, max:number) : number {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

// remove popup from array
function closePopup(index: number) {
    popups.value.splice(index, 1);
    // check if no popups left after closing
    if (popups.value.length === 0) {
        stopStopwatch()
    }
}

let intervalId: number | undefined

function stopStopwatch() {
    if (intervalId !== undefined) {
        clearInterval(intervalId)
        intervalId = undefined
    }
}

function startStopwatch() {
    intervalId = setInterval(() => {
        time.value++
    }, 10)
}

const formattedTime = computed(() => {
    const milliseconds = time.value % 100
    const seconds = Math.floor(time.value / 100) % 60
    const minutes = Math.floor(time.value / 6000) % 60
    return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}:${String(milliseconds).padStart(2, '0')}`
})

onMounted(() => {
    generatePopups()
    startStopwatch()
    console.log('generated!')
})

</script>