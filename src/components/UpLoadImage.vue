<template>
    <div class="root" :style="{
      '--width': props?.width,
      '--primaryColor': props?.primaryColor,
      '--unSelectColor': props.unSelectColor,
      '--mainMinHeight': props.mainMinHeight,
      '--mainHeight': props.mainHeight,
      '--iconWidth':props.iconWidth,
      '--imgItemWidth':'70px',
      '--imgItemHeight':'70px',
    }">
        <input class="file-upload" type="file" accept="image/*" style="display: none" @change="upLoadImage"
            ref="fileUpLoad" multiple />
        <div class="upLoadPicture">
            <div class="main" :class="{ mainHover: isMianHover }" @click="upLoad" @dragover="dragOverHandler"
                @drop="dropHandler" @dragend="dragEndHandler" @dragenter="dragEnterHandler"
                @dragleave="dragLeaveHandler" ref="main">
                <img src="../assets/upload.svg" id="upLoadImg" draggable="false" />
            </div>
            <div class="tip">
                <img src="../assets/alert.svg" draggable="false" />
                <div class="text">上传文件大小不能超过500kb</div>
            </div>
            <div class="imageList" v-for="(item, index) in previewImgList">
                <div class="imageItem">
                    <img class="img" :src="item?.src" :width="item?.width" :height="item?.height" />
                    <div class="title">{{ item?.title }}</div>
                    <div class="success">
                        <img id="successImg" src="../assets/success.svg" />
                    </div>
                    <div class="close" @click="deleteImg(index)">
                        <svg class="icon" aria-hidden="true" id="closeImg">
                            <use xlink:href="#icon-close"></use>
                        </svg>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- <ViewPicture></ViewPicture> -->
</template>

<script setup>
import { ref } from "vue";

const props = defineProps({
    width: {
        type: String,
        required: false,
        default: "60vw",
    },
    unSelectColor: {
        type: String,
        required: false,
        default: "#ccc",
    },
    primaryColor: {
        type: String,
        required: false,
        default: "#1296db",
    },
    mainMinHeight: {
        type: String,
        required: false,
        default: "150px",
    },
    mainHeight: {
        type: String,
        required: false,
        default: "200px",
    },
    iconWidth: {
        type: String,
        required: false,
        default: "100px",
    },
    imgItemHeight: {
        type: String,
        required: false,
        default: "70px",
    },
    imgItemWidth: {
        type: String,
        required: false,
        default: "70px",
    }
});

// console.log(props.imgItemWidth);
// console.log(props.imgItemHeight);

const main = ref();
const fileUpLoad = ref();
const previewImgList = ref([]);
const isMianHover = ref(false);

const upLoad = () => {
    fileUpLoad.value.click();
};

const upLoadImage = () => {
    const target = fileUpLoad.value.files;
    addImg(target);
};

const deleteImg = (index) => {
    // console.log(index);
    if (previewImgList.length - 1 < index) {
        return;
    }
    previewImgList.value.splice(index, 1);
};

const addImg = (files) => {
    for (let i = 0; i < files.length; i++) {
        let reader = new FileReader();
        const element = files[i];
        reader.readAsDataURL(element);
        reader.onload = function (evt) {
            // console.log(evt.target.result);
            let img = new Image();
            img.title = element.name;
            img.src = evt.target.result;
            img.onload = function () {
                let wTOh = img.width / img.height;
                img.height = Number.parseFloat(props.imgItemHeight);
                img.width = img.height * wTOh;
                previewImgList.value.push(img);
            };
        };
    }
};

const dropHandler = (e) => {
    e.stopPropagation();
    //阻止浏览器默认打开文件的操作
    e.preventDefault();
    let target = e.dataTransfer.files;
    isMianHover.value = false;
    addImg(target);
};

const dragEndHandler = (e) => {
    isMianHover.value = false;
    e.stopPropagation();
    e.preventDefault();
};

const dragEnterHandler = (e) => {
    isMianHover.value = true;
    e.stopPropagation();
    e.preventDefault();
};

const dragLeaveHandler = (e) => {
    isMianHover.value = false;
    e.stopPropagation();
    e.preventDefault();
};

const dragOverHandler = (e) => {
    isMianHover.value = true;
    e.stopPropagation();
    //阻止浏览器默认打开文件的操作
    e.preventDefault();
    e.dataTransfer.dropEffect = "move";
};
</script>

<style lang="scss">
@import "../style/UpLoadImage.scss";
</style>
