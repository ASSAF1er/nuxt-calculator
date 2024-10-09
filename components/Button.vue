<script setup lang="ts">

const props = defineProps<{
    customClass?: string,
    value?: string | number,
    onclick?: ()=> void
}>()
const userInput=useState('userInput')

function updateUserInput(value){
    userInput.value=userInput.value+value
}

</script>

<template>
    <button
    @click="props.onclick?props.onclick() : updateUserInput(value)"
        :class="[props.customClass, 'flex-1 flex items-center justify-center bg-[#22222a] rounded-full shadow-sm active:scale-90  active:bg-primary hover:bg-white/20 duration-500 ease-in-out']">
        <span class="text-2xl text-slate-200  ">
        <slot/>
    </span>
</button></template>