<script setup>
import EventCard from '@/components/EventCard.vue'
import BookingItem from './components/BookingItem.vue'
import { ref, onMounted } from 'vue'
import LoadingEvenCard from './components/LoadingEvenCard.vue'

const events = ref([])
const loading = ref(false)
const error = ref(null)
const bookings = ref([])
const bookingLoading = ref(false)
const bookingError = ref(null)

const fetchEvents = async () => {
  loading.value = true
  try {
    const response = await fetch('http://localhost:3001/events')
    if (!response.ok) {
      throw new Error('Failed to fetch events!')
    }
    const result = await response.json()
    events.value = result
  } catch (error) {
    error.value = error.message
  } finally {
    loading.value = false
  }
}

const handleRegistration = async (event) => {
  const newBooking = {
    id: Date.now().toString(),
    userId: 1,
    eventId: event.id,
    eventTitle: event.title,
    status: 'pending',
  }

  bookings.value.push(newBooking)

  try {
    const response = await fetch('http://localhost:3001/bookings', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ ...newBooking, status: 'confirmed' }),
    })

    if (response.ok) {
      const index = bookings.value.findIndex((b) => b.id === newBooking.id)
      bookings.value[index] = await response.json()
    }
  } catch (error) {
    // handle error
  }
}

const fetchBooking = async () => {
  bookingLoading.value = true
  try {
    const response = await fetch('http://localhost:3001/bookings')
    if (!response.ok) {
      throw new Error('Failed to fetch bookinds')
    }
    const result = await response.json()
    bookings.value = result
  } catch (err) {
    bookingError.value = err.message
  } finally {
    bookingLoading.value = false
  }
}

onMounted(() => {
  fetchEvents()
  fetchBooking()
})
</script>

<template>
  <main class="container mx-auto my-8 space-y-8">
    <h1 class="text-4xl font-medium">Event Booking App</h1>
    <h2 class="text-2xl font-medium">All Events</h2>

    <!-- <h2 v-else-if="error">ERROR: {{ error }}</h2> -->

    <section class="grid grid-cols-2 gap-8">
      <template v-if="!loading">
        <EventCard
          v-for="event in events"
          :key="event.id"
          :title="event.title"
          :when="event.when"
          :description="event.description"
          @register="handleRegistration(event)"
        />
      </template>
      <template v-else>
        <LoadingEvenCard v-for="i in 4" :key="i" />
      </template>
    </section>
    <h2 class="text-2xl font-medium">Your Bookings</h2>
    <p v-if="bookings.length === 0">No booking available!</p>
    <section v-else class="grid grid-cols-1 gap-5">
      <BookingItem
        v-for="booking in bookings"
        :key="booking.id"
        :title="booking.eventTitle"
        :status="booking.status"
      />
    </section>

    <div class="my-5 w-[30%] p-5 bg-white">
      <h2 class="text-xl font-semibold pb-3">Muhammad Razaullah</h2>
      <p>I am a Full Stack Developer, Currently working in Nisum Consulting Pvt Ltd</p>
      <button class="px-3 py-2 text-white mt-4 bg-slate-500">Know More</button>
    </div>
  </main>
</template>
