<template>
    <section class="w-screen h-screen bg-primary py-8 flex items-center justify-center">
        <div class="text-primary flex flex-col items-center">
            <div class="flex items-center gap-5">
                <img class="w-14" src="../assets/icons/blink.png" alt="blink" />
                <span class="text-[60px]">Your Result</span>
                <img class="w-14" src="../assets/icons/blink.png" alt="blink" />
            </div>
            <p class="text-[40px]">Time: {{ props.time.toFixed(0) }}</p>

            <span @click="handleOnReplay" class="text-[30px] hover:underline cursor-pointer">Re play</span>
        </div>
    </section>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

const emit = defineEmits(['onReplay']);

const handleOnReplay = () => {
    emit('onReplay');
};

const props = defineProps({
    time: Number,
});
</script>

<style lang="scss" scoped></style>
