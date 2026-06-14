<script setup>
import RoundButton from './RoundButton.vue'
import SectionCard from './SectionCard.vue'

defineProps({
  title: String,
  status: String,
})
</script>

<template>
  <SectionCard>
    <div class="flex justify-between">
      <div class="flex space-x-2">
        <div class="">{{ title }}</div>
        <div :class="status === 'pending' ? 'text-yellow-500' : 'text-green-500'">{{ status }}</div>
      </div>
      <RoundButton variant="danger">Cancel</RoundButton>
    </div>
  </SectionCard>
</template>
