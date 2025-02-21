<template>
    <!-- Screen + background -->
    <div class="w-screen h-screen bg-gray-100">
        <div v-for="popup in popups">
            <Popup :x="popup.x" :y="popup.y"  :width="popup.width" :height="popup.height"/>
        </div>
    </div>
</template>
<script setup lang="ts">
interface Popup {
    x: number 
    y: number
    width: number
    height: number
}
const popups = ref<Popup[]>([])
const screenX = 1920 // fetch dynamicly
const screenY = 1080

for (let i = 0; i < 10; i++) {
    const popup: Popup = {
        width: randIntInclusive(100, 500),
        height: randIntInclusive(100, 500),
        x: randIntInclusive(0, screenX),
        y: randIntInclusive(0, screenY)
    }
    popups.value.push(popup);
}


function randIntInclusive(min: number, max:number) : number {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min + 1)) + min;
}
</script>