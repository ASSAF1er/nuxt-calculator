<script setup lang="ts">
    const props = defineProps<{
    date:string,
    expression:string,
    result: number,
    }>()
</script>

<template>

    <div class="px-5 py-3 border-t border-gray-700">
        <p class="text-slate-300 mb-5"> {{ props.date }} </p>
        <p class="text-slate-200 text-xl text-right font-500"> {{ props.expression }}</p>
        <p class="text-slate-300 text-xl text-right font-500"> {{ Number.isInteger(props.result)? props.result : parseFloat(props.result?.toFixed(3)) }}</p>
    </div>

</template>