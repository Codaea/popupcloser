<template>
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
}

onMounted(() => {
    generatePopups()
    console.log('generated!')
})

</script>