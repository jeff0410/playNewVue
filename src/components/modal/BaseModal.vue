<template>
  <div v-if="isOpen" class="modal-overlay" @click.self="closeModal">
    <div class="modal-content">
      <div class="modal-header">
        <slot name="header"></slot>
      </div>
      <div class="modal-body">
        <slot></slot>
      </div>
      <div class="modal-footer">
        <button @click="closeModal">Cancel</button>
        <button @click="confirmAction">Confirm</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, onMounted, onBeforeUnmount } from "vue";

const props = defineProps({
  isOpen: Boolean,
});

const emits = defineEmits(["close"]);

const closeModal = () => {
  emits("close");
};

const onEscKey = (event) => {
  if (event.key === "Escape") {
    closeModal();
  }
};

onMounted(() => {
  document.addEventListener("keydown", onEscKey);
});

onBeforeUnmount(() => {
  document.removeEventListener("keydown", onEscKey);
});
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.4);
  display: flex;
  justify-content: flex-end;
  z-index: 999;
  transition: opacity 1s ease;
}

.modal-content {
  width: 600px;
  height: 100%;
  background: white;
  box-shadow: -2px 0 10px rgba(0, 0, 0, 0.1);
  transform: translateX(100%);
  transition: transform 1s ease;
}

.modal-overlay .modal-content {
  transform: translateX(0);
}

.modal-header {
  height: 40px;
  display: flex;
  align-items: center;
  padding: 30px;
  font-weight: bold;
  font-size: 24px;
}

.modal-body {
  flex: 1;
  padding: 30px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.modal-footer {
  position: fixed;
  bottom: 0;
  right: 0;
  padding: 30px;
  gap: 10px;
}
</style>
