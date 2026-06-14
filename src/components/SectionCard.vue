<template>
  <RoundedCard>
    <h3 v-if="$slots.header" class="p-4 font-medium text-xl border-b border-gray-200">
      <slot name="header"></slot>
    </h3>
    <div v-if="$slots.default" class="p-4 border-b border-gray-200">
      <slot></slot>
    </div>
    <div v-if="$slots.footer">
      <slot name="footer"></slot>
    </div>
  </RoundedCard>
</template>

<script setup>
import RoundedCard from './RoundedCard.vue'
</script>
