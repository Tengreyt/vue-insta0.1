<template>
    <div class="flex justify-center inset-x-0  mt-24 z-60">
        <button 
        class="bg-blue-700 hover:bg-blue-800 text-white font-bold py-2 px-4 rounded" 
        @click="addParagraph">Сколько раз нажали: {{ count }}</button>
    </div>
</template>

<script setup>
import { ref } from 'vue'

const count = ref(0)
const items = ref([])

const addParagraph = () => {
  count.value++
  items.value.push(`Это клик ${count.value}`)
}
</script>