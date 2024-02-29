<template>
    <HomePage v-if="page === 'default'" @onLevel="handleLevel" />
    <GamePage v-if="page === 'game'" :data="data" @onResult="handleResult" />
    <ResultPage v-if="page === 'result'" :time="30.235" @onReplay="handleReplay" />
</template>

<script setup>
import { ref } from 'vue';
import HomePage from '@/pages/HomePage.vue';
import GamePage from '@/pages/GamePage';
import ResultPage from '@/pages/ResultPage';

const page = ref('default');
const timeResult = ref();
let data = null;

const handleLevel = (item) => {
    page.value = 'game';
    data = item;
};

const handleResult = (time) => {
    page.value = 'result';
    timeResult.value = time;
};

const handleReplay = () => {
    page.value = 'default';
};
</script>
