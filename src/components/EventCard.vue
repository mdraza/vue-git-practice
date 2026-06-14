<template>
  <SectionCard>
    <!-- <h3 class="p-4 font-medium text-xl border-b border-gray-200"></h3> -->
    <template #header>
      {{ title }}
    </template>
    {{ when }}
    <template #footer>
      <p class="p-4">{{ description }}</p>
      <section class="flex justify-end p-4">
        <RoundButton label="Register" @click="$emit('register')">Register</RoundButton>
      </section>
    </template>
  </SectionCard>
</template>

<script setup>
import RoundButton from './RoundButton.vue'
import SectionCard from './SectionCard.vue'

defineProps({
  title: String,
  when: String,
  description: String,
})

defineEmits(['register'])
</script>
