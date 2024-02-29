<script setup>
import { reactive, defineEmits } from 'vue';

const levels = reactive([
    {
        title: 'easy',
        x: 4,
        y: 4,
    },
    {
        title: 'normal',
        x: 6,
        y: 6,
    },
    {
        title: 'hard',
        x: 8,
        y: 8,
    },
    {
        title: 'super hard',
        x: 10,
        y: 10,
    },
]);

const emit = defineEmits(['onLevel']);

const handleLevel = (item) => {
    emit('onLevel', item);
};
</script>

<template>
    <div class="h-screen w-screen bg-primary flex flex-col items-center justify-center text-primary">
        <h1 class="text-[4.5rem]">POKE MEMORIES</h1>
        <span class="text-[2rem]">Select mode to start game</span>

        <div class="flex items-center gap-5 mt-5">
            <div
                v-for="level in levels"
                :key="level.title"
                @click="() => handleLevel(level)"
                class="w-[150px] h-[150px] cursor-pointer hover:bg-white hover:text-black rounded-2xl border select-none border-primary flex flex-col items-center justify-center transition-all ease-linear"
            >
                <h4 class="text-[2rem]">{{ `${level.x} x ${level.y}` }}</h4>
                <p class="capitalize text-xl">{{ level.title }}</p>
            </div>
        </div>
    </div>
</template>

<style></style>
