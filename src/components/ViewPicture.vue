<template>
    <div class="modal">
        <div class="close">
            <img class="closeIcon" draggable="false" src="../assets/close.svg" width="20">
        </div>
        <div class="imgShow">
            <img src="../assets/upload.svg" />
        </div>
        <div class="tool">
            <div class="magnify"></div>
            <div class="shrink"></div>
            <div class="toLeft"></div>
            <div class="toRight"></div>
        </div>
    </div>
</template>

<script setup>
import { onBeforeMount } from 'vue';

onBeforeMount(() => {
    console.log(window.client);
})

const displayWindowSize = () => {
    const width = document.documentElement.clientWidth
    const height = document.documentElement.clientHeight
    console.log(width);
    console.log(height);
}

window.onresize = () => {
    displayWindowSize()
}
</script>

<style scoped>
@import "../style/ViewPicture.scss"
</style>