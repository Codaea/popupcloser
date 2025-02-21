<template>
  <div>
 <!-- popup window -->
        <div class="absolute bg-gray-400 border border-black" 
            :style="{top: y + 'px', left: x + 'px', width: props.width + 'px', height: props.height + 'px'}" @mousedown="startDrag">
            <div class="w-full h-8 bg-gray-600 flex items-center relative">
                <p>POPUP WINDOW</p>
                <button class="absolute top-1 right-2 transition-all hover:underline hover:text-red-500" @click="emit('close')">X</button>
            </div>
        </div>   
  </div>
</template>

<script lang="ts" setup>
const props = defineProps({
    x: {
        type: Number,
        required: true
    },
    y: {
        type: Number,
        required: true
    },
    width: Number,
    height: Number,
    color: {
        type: String,
        required: true
    }
})

const emit = defineEmits(['close']);

// x and y from props are only used to initialize the ref values
const x = ref(props.x);
const y = ref(props.y);
// offset allows to drag the popup window from the current mouse position instead of teleporting window to top left corner
const offSetX = ref(0);
const offSetY = ref(0);

const isDragging = ref(false);

function startDrag(e: MouseEvent) {
    isDragging.value = true;
    offSetX.value = e.clientX - x.value;
    offSetY.value = e.clientY - y.value;

    window.addEventListener('mousemove', dragEl);
    window.addEventListener('mouseup', stopDrag);
}

function stopDrag() {
    isDragging.value = false;
    window.removeEventListener('mousemove', dragEl);
    window.removeEventListener('mouseup', stopDrag);
}

function dragEl(e: MouseEvent) {
    if (isDragging.value) {
        x.value = e.clientX - offSetX.value;
        y.value = e.clientY - offSetY.value;
    }
}

</script>
