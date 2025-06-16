<script setup>
import { ref, onMounted, onUnmounted, watch } from 'vue';
import IconCancel from "../../assets/IconCancel.vue";
import {truncateText} from "../../utils/helpers.js";
import "./DisplayMessages.styles.css";

const props = defineProps({
  messages: {
    type: Array,
    required: true
  }
});
const emit = defineEmits(['delete']);

const currentMessage = ref(null);
const currentIndex = ref(0);
const delay = 3000;
let timer = null;

const showNextMessage = () => {
  if (props.messages.length === 0) {
    currentMessage.value = null;
    return;
  }

  if (currentIndex.value >= props.messages.length) {
    currentIndex.value = 0;
  }

  currentMessage.value = props.messages[currentIndex.value];
};

const startTimer = () => {
  clearInterval(timer);
  timer = setInterval(() => {
    currentIndex.value++;
    showNextMessage();
  }, delay);
};

const handleDelete = () => {
  if (!currentMessage.value) return;

  const deletedId = currentMessage.value.id;
  emit('delete', deletedId);
  const deletedIndex = props.messages.findIndex(m => m.id === deletedId);

  if (deletedIndex !== -1) {
    if (currentIndex.value > deletedIndex) {
      currentIndex.value--;
    }
  }
  clearInterval(timer);
  showNextMessage();
  startTimer();
};

onMounted(() => {
  showNextMessage();
  startTimer();
});

onUnmounted(() => clearInterval(timer));

watch(() => props.messages, () => {
  if (currentMessage.value && !props.messages.some(m => m.id === currentMessage.value.id)) {
    handleDelete();
  } else {
    showNextMessage();
  }
}, { deep: true });
</script>

<template>
  <div class="display-container">
    <transition name="fade" mode="out-in">
      <div v-if="currentMessage" :key="currentMessage.id" class="display-message">
        <button @click="handleDelete" class="delete-btn" aria-label="Удалить сообщение">
          <IconCancel :size=24 color="#2C674D"/>
        </button>
        <p class="display-text">{{ truncateText(currentMessage.text) }}</p>
      </div>
      <div v-else key="no-messages" class="no-messages">
        Нет сообщений для показа
      </div>
    </transition>
  </div>
</template>
