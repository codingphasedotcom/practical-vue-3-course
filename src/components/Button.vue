<script setup>
import { ref, computed } from "vue";

const emit = defineEmits(["click"]);
const props = defineProps({
  variant: { type: String, default: "primary" },
  size: { type: String, default: "medium" },
  loading: { type: Boolean, default: false },
  disabled: { type: Boolean, default: false },
});

const variantClasses = computed(() => {
  switch (props.variant) {
    case "secondary":
      return "bg-gray-500 text-white hover:bg-gray-600";
    case "danger":
      return "bg-red-500 text-white hover:bg-red-600";
    default:
      return "bg-blue-500 text-white hover:bg-blue-600";
  }
});

const sizeClasses = computed(() => {
  switch (props.size) {
    case "small":
      return "text-sm";
    case "large":
      return "text-lg";
    default:
      return "text-base";
  }
});
const handleClick = () => {
  if (!props.loading && !props.disabled) {
    emit("click", {
      button: props.variant,
      size: props.size,
      loading: props.loading,
      disabled: props.disabled,
      data: 21,
    });
    console.log("child: Clicked Button");
  } else {
    console.log("child: button disabled or loading right now");
  }
};
</script>

<template>
  <button
    :class="[
      'px-4 py-2 rounded font-medium transition',
      variantClasses,
      sizeClasses,
      { 'opacity-50 cursor-not-allowed': loading || disabled },
    ]"
    @click="handleClick"
  >
    <slot></slot>
    Testing
  </button>
</template>

<style scoped></style>
