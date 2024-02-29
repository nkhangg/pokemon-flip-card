<template>
    <div :class="['card', { disabled }]">
        <div class="card__inner" :class="{ 'is-flipped': isFlipped }" @click="onToggleFlipCard">
            <div class="card__face card__face--front">
                <div class="card__content"></div>
            </div>
            <div ref="refCardFace" class="card__face card__face--back">
                <div class="card__content"></div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, defineProps, defineEmits, onMounted, defineExpose } from 'vue';

const props = defineProps({
    imgBackFaceUrl: String,
    data: Object,
});

const isFlipped = ref(false);
const refCardFace = ref(null);
const disabled = ref(false);
const imgBackFaceUrl = ref(props.imgBackFaceUrl);

const emit = defineEmits(['onChoose']);

const onToggleFlipCard = () => {
    if (!disabled.value) {
        isFlipped.value = !isFlipped.value;

        emit('onChoose', props.data);
    }
};

const onOpenFlipCard = () => {
    isFlipped.value = true;
    disabled.value = true;
};
const onCloseFlipCard = () => {
    isFlipped.value = false;
};

defineExpose({
    onOpenFlipCard,
    onCloseFlipCard,
});

onMounted(() => {
    refCardFace.value.style.backgroundImage = `url('${require('@/assets/images/' + imgBackFaceUrl.value)}')`;
});
</script>

<style scoped>
.card {
    display: inline-block;
    max-height: 198px;
}

.card__inner {
    width: 100%;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;
}

.card.disabled .card__inner {
    cursor: default;
}

.card__inner.is-flipped {
    transform: rotateY(-180deg);
}

.card__face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    overflow: hidden;
    border-radius: 1rem;
    padding: 1rem;
    box-shadow: 0 3px 18px 3px rgba(0, 0, 0, 0.2);
}

.card__face--front .card__content {
    background: url('../assets/images/icon_back.png') no-repeat center center;
    background-size: 50%;
    height: 100%;
    width: 100%;
}

.card__face--back {
    background-color: var(--light);
    transform: rotateY(-180deg);
    background-position: center center;
    background-repeat: no-repeat;
    background-size: contain;
    background-size: 60%;
    height: 100%;
    width: 100%;
}
</style>
