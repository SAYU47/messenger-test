<script setup>
import IconCancel from "../../assets/IconCancel.vue";
import {formatDateTime, truncateText} from "../../utils/helpers.js";
import "./Message.styles.css";

defineProps({
  id: {
    type: Number,
    required: true
  },
  text: {
    type: String,
    required: true
  },
  deleted:{
    type: Boolean,
    required: true
  },
  created:{
    type: Date,
    required: true
  },

});

</script>

<template>
  <div class="message">
    <div class="message-icon">
      <IconCancel v-if="deleted" :size="16" />
    </div>
    <span class="message-text">{{ truncateText(text) }}</span>
    <time class="message-date" :datetime="created.toISOString()">
      {{ formatDateTime(created) }}
    </time>
  </div>
</template>
