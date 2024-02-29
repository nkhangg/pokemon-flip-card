<template>
    <section class="w-screen h-screen bg-primary py-8 flex items-center justify-center">
        <div class="fixed top-0 right-0 text-primary p-9 text-2xl">
            <span>{{ countTime }} S</span>
        </div>

        <div ref="container" class="grid w-full h-full max-w-[862px] gap-4">
            <Card ref="refCard" v-for="(item, index) in resultsCards" :key="index" :imgBackFaceUrl="`${item}.png`" :data="{ index, data: item }" @onChoose="handleChoose" />
        </div>
    </section>
</template>

<script setup>
import { defineProps, onMounted, ref, defineEmits, watch } from 'vue';

import { shuffled } from '@/ultils/array';
import { delay } from '@/ultils/fn';
import Card from '../components/Cart.vue';

const container = ref();
const validArray = ref([]);
const resultsCards = ref([]);
const countTime = ref(0);
const result = ref(0);
const refCard = ref();

const startTime = new Date();

const emit = defineEmits(['onResult']);

const props = defineProps({
    data: Object,
});

const countTimeFn = () => {
    var startTime = performance.now();
    setInterval(() => {
        var endTime = performance.now();

        countTime.value = ((endTime - startTime) / 1000).toFixed(0);
    }, 1000);
};

onMounted(countTimeFn);

const handleChoose = async (data) => {
    validArray.value.push(data);
    if (validArray.value.length >= 2) {
        if (validArray.value[0].data === validArray.value[1].data) {
            validArray.value.forEach((item) => refCard.value[item.index].onOpenFlipCard());
            result.value += 1;

            if ((props.data.x * props.data.y) / 2 === result.value) {
                const endTime = new Date();
                await delay(200);
                emit('onResult', (endTime.getTime() - startTime.getTime()) / 1000);
            }
        } else {
            await delay(800);
            validArray.value.forEach((item) => {
                refCard.value[item.index].onCloseFlipCard();
            });
        }

        validArray.value = [];

        return;
    }
};

watch(resultsCards, () => {
    console.log(resultsCards.value);
});

const handleInit = () => {
    container.value.style.gridTemplateColumns = `repeat(${props.data.x}, minmax(0, 1fr))`;
    container.value.style.gridTemplateRows = `repeat(${props.data.y}, minmax(0, 1fr))`;

    const firstCards = Array.from({ length: (props.data.x * props.data.y) / 2 }, (_, i) => i + 1);

    const sercondCards = [...firstCards];

    resultsCards.value = shuffled(shuffled([...firstCards, ...sercondCards]));
};

onMounted(handleInit);
</script>

<style>
.product {
    transform-style: preserve-3d;
}

.product__front {
    background-image: url('../assets/images/icon_back.png');
    background-size: 50%;
    background-repeat: no-repeat;
    background-position: center;
    box-shadow: 0 3px 18px 3px rgba(0, 0, 0, 0.2);
}

.product__front:hover {
    cursor: pointer;
    transform: rotateY(-180);
}
</style>
