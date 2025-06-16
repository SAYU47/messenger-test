<script setup>
import IconSendMessage from "../../assets/IconSendMessage.vue";
import './MessageForm.styles.css'
const props = defineProps({
  modelValue: {
    type: String,
    default: ''
  },
  disabled: {
    type: Boolean,
    default: false
  }
});

const emit = defineEmits(['update:modelValue', 'submit']);

const handleSubmit = () => {
  const trimmedValue = props.modelValue;
  if (props.modelValue && !props.disabled) {
    emit('submit', {
      text: trimmedValue,
      created: new Date()
    });
    emit('update:modelValue', '');
  }
};
</script>

<template>
  <div class="form-floating message-form d-flex gap-3">
    <input
        :value="modelValue"
        @input="emit('update:modelValue', $event.target.value)"
        @keyup.enter="handleSubmit"
        placeholder="Новое сообщение"
        class="form-control"
        id="floatingInput"
    />
    <label class="form-control-label" for="floatingInput">Новое сообщение</label>
    <button
        @click="handleSubmit"
        class="btn btn-success send-button border-0"
    >
      <span class="send-text d-sm-inline">Отправить</span>
      <IconSendMessage class="send-icon"/>
    </button>
  </div>
</template>


