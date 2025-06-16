<script setup lang="ts">
import { ref, computed } from 'vue';
import Message from "../Message/Message.vue";
import MessageForm from "../MessageForm/MessageForm.vue";
import MessageDisplay from "../MessageDisplay/MessageDisplay.vue";
import './MessageList.styles.css'

interface Message {
  id: number;
  text: string;
  isDeleted: boolean;
  created: Date;
}
const messages = ref<Message[]>([
  { id: 1, text: 'Сообщение 1', isDeleted: false, created: new Date() },
  { id: 2, text: 'Сообщение 2', isDeleted: false, created: new Date() },
  { id: 3, text: 'Сообщение 3', isDeleted: false, created: new Date() },
  { id: 4, text: 'Сообщение 4', isDeleted: false, created: new Date() },
  { id: 5, text: 'Сообщение 5', isDeleted: false, created: new Date() },
  { id: 6, text: 'Сообщение 6', isDeleted: false, created: new Date() },
]);

const newMessage = ref('');

const activeMessages = computed(() =>
    messages.value.filter(msg => !msg.isDeleted)
);

const addMessage = ({ text, created }) => {
  if (!text.trim()) return;

  messages.value.push({
    id: Date.now(),
    text: text.trim(),
    isDeleted: false,
    created: created || new Date()
  });
}

const deleteMessage = (id) => {
  const message = messages.value.find(msg => msg.id === id);
  if (message) message.isDeleted = true;
}
</script>

<template>
  <div class="message-list container-md p-4 bg-white rounded shadow">
    <MessageDisplay
        :messages="activeMessages"
        class="message-display mb-4 p-4 bg-success bg-opacity-10 rounded"
        @delete="deleteMessage"
    />

    <ul class="messages list-group rounded">
      <Message
          v-for="message in messages"
          :key="message.id"
          :id="message.id"
          :text="message.text"
          :created="message.created"
          :deleted="message.isDeleted"
          @delete="deleteMessage(message.id)"
      />
    </ul>

    <!-- Message Form -->
    <MessageForm
        v-model="newMessage"
        @submit="addMessage"
        class="mt-3"
    />
  </div>
</template>