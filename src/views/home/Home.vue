<template>
  <div>
    <button @click="openModal">모달</button>
    <BaseModal :isOpen="isModalOpen" @close="closeModal">
      <template #header>
        <div>Title</div>
      </template>
      <div class="content-header">
        <span>first</span>
        <span>|</span>
        <span>next1</span>
        <span>|</span>
        <span>next2</span>
        <span>|</span>
        <span>next3</span>
        <span>|</span>
        <span>next4</span>
      </div>
      <div class="file-list">
        <div v-for="file in files" :key="file.name">{{ file.name }}</div>
      </div>
      <div class="file-dropzone" @drop.prevent="handleDrop" @dragover.prevent>
        <button @click="handleFileClick">파일 추가</button>
      </div>
    </BaseModal>
    <input
      type="file"
      ref="fileInput"
      @change="handleFileChange"
      style="display: none"
    />
  </div>
</template>

<script setup>
import { ref } from "vue";
import BaseModal from "@/components/modal/BaseModal.vue";

const isModalOpen = ref(false);
const files = ref([]);

const openModal = () => {
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
};

const handleFileClick = () => {
  const fileInput = document.querySelector('input[type="file"]');
  fileInput.click();
};

const handleFileChange = (event) => {
  const selectedFiles = Array.from(event.target.files);
  files.value = files.value.concat(selectedFiles);
};

const handleDrop = (event) => {
  const droppedFiles = Array.from(event.dataTransfer.files);
  files.value = files.value.concat(droppedFiles);
};
</script>

<style scoped>
.content-header {
  display: flex;
  align-items: center;
  gap: 5px;
}

.file-list {
  display: flex;
  flex-wrap: wrap;
  gap: 4px;
}

.file-dropzone {
  border: 2px dashed #ddd;
  width: 100%;
  height: 250px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}
</style>
