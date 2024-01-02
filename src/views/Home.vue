<template>
    <div class="main-home-div">
        <div class="feature-container-mobile">
            <div class="selection-circle-div" :class="`state-${stateCircleSelectionFeature}`">

            </div>
            <button :disabled="activeBrushing || activeCroping" class="btn" @click="openUploadFileModal">
                <!-- <div class="tooltip-div">Upload</div> -->
                <img src="@/assets/images/icons/upload.svg" alt="Upload">
            </button>
            <button :disabled="(activeBrushing) || selectedFileImageForEdit == undefined" class="btn"
                @click="callEnableCroping">
                <img src="@/assets/images/icons/crop.svg" alt="Crop">
            </button>
            <button :disabled="(activeCroping) || selectedFileImageForEdit == undefined" class="btn"
                @click="callEnablePainting">
                <img src="@/assets/images/icons/paint.svg" alt="Paint">
            </button>
            <button :disabled="(activeBrushing || activeCroping) || selectedFileImageForEdit == undefined" class="btn"
                @click="callDownLoadImage">
                <img src="@/assets/images/icons/download.svg" alt="Download">
            </button>
        </div>
        <div class="container-button-editor">
            <div class="editor-image-div">
                <ImageEditor @finish-editing="finishEditImage" :max-height="450" :max-width="450" ref="imageEditor"
                    background-crop-div-color="white" border-crop-div-color="#4286f4" :color-brush="colorBrush"
                    v-model:file-image="selectedFileImageForEdit" />
            </div>
            <div class="feature-buttons">
                <button :disabled="activeBrushing || activeCroping" @click="openUploadFileModal" class="btn">
                    <input ref="inputUploadFile" type="file" @change="loadImageFile">
                    <img src="@/assets/images/icons/upload.svg" alt="Upload">
                    Upload
                </button>
                <button :disabled="(activeBrushing || activeCroping) || selectedFileImageForEdit == undefined"
                    @click="callEnableCroping" class="btn">
                    <img src="@/assets/images/icons/crop.svg" alt="Crop">
                    Crop
                </button>
                <button :disabled="(activeBrushing || activeCroping) || selectedFileImageForEdit == undefined"
                    @click="callEnablePainting" class="btn">
                    <img src="@/assets/images/icons/paint.svg" alt="Paint">
                    Paint
                </button>
                <button :disabled="(activeBrushing || activeCroping) || selectedFileImageForEdit == undefined"
                    @click="callDownLoadImage" class="btn">
                    <img src="@/assets/images/icons/download.svg" alt="Download">
                    Download
                </button>
            </div>
            <div class="control-buttons" v-if="activeBrushing || activeCroping">
                <button @click="callCancelChanges" class="btn">
                    Cancel
                </button>
                <button @click="callSaveChanges" class="btn">
                    Save
                </button>
            </div>
        </div>
    </div>


    <!--<div class="options-paiting" v-if="activeBrushing">
        <input type="color" v-model="colorBrush">
    </div>-->
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { ImageEditor } from "vue3-image-editor"
import "vue3-image-editor/styles.css"

const colorBrush = ref<string>("")
const selectedFileImageForEdit = ref<File>()
const imageEditor = ref<any>(null)
const activeBrushing = ref<boolean>(false)
const activeCroping = ref<boolean>(false)
const inputUploadFile = ref<any>(null)
const stateCircleSelectionFeature = ref<number>(0)

onMounted(() => {
})

function openUploadFileModal() {
    inputUploadFile.value.click()
    stateCircleSelectionFeature.value = 1
}

function loadImageFile(event: Event) {
    selectedFileImageForEdit.value = (event?.target as any).files[0]
    stateCircleSelectionFeature.value = 0
}


function callEnableCroping() {
    imageEditor.value.enableCroping()
    activeCroping.value = true
    stateCircleSelectionFeature.value = 2
}

function callEnablePainting() {
    imageEditor.value.enablePainting()
    activeBrushing.value = true
    stateCircleSelectionFeature.value = 3
}

function callFinishEditing() {
    imageEditor.value.finishEditing()
}

function finishEditImage(newFile: File) {
    console.log("Final File After Change", newFile)
}

function callDownLoadImage() {
    imageEditor.value.download()
    stateCircleSelectionFeature.value = 4
    setTimeout(() => {
        stateCircleSelectionFeature.value = 0
    }, 2000);
}

function callSaveChanges() {
    imageEditor.value.saveChanges()
    activeBrushing.value = false
    activeCroping.value = false
    stateCircleSelectionFeature.value = 0
}

function callCancelChanges() {
    imageEditor.value.cancelChanges()
    activeBrushing.value = false
    activeCroping.value = false
    stateCircleSelectionFeature.value = 0
}

</script>

<style scoped lang="scss">
@import './Home.scss'
</style>