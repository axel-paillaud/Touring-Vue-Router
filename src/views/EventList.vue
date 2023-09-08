<template>
  <h1>Events for Good</h1>
  <div class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event" />

    <div class="pagination">
      <div>
        <router-link
          :to="{ name: 'EventList', query: { page: page - 1 } }"
          rel="prev"
          id="page-prev"
          v-if="page != 1"
          >&#60; Previous</router-link
        >
      </div>

      <ul class="pagination-list-container">
        <li v-for="n in totalPages" :key="n">
          <router-link :to="{ name: 'EventList', query: {page: n} }">{{ n }}</router-link>
        </li>
      </ul>

      <div>
        <router-link
          :to="{ name: 'EventList', query: { page: page + 1 } }"
          rel="next"
          id="page-next"
          v-if="hasNextPage"
          >Next &#62;</router-link
        >
      </div>
    </div>
  </div>
</template>

<style scoped>
ul {
  padding: 0;
  list-style-type: none;
  margin: 0;
}

.pagination-list-container {
  display: flex;
  gap: 4px;
}
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.pagination {
  display: flex;
  width: 290px;
  justify-content: space-between;
}
.pagination a {
  text-decoration: none;
  color: #2c3e50;
}

#page-prev {
  text-align: left;
}

#page-next {
  text-align: right;
}
</style>

<script>
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService.js'
import { watchEffect } from 'vue'

export default {
  name: 'EventList',
  props: ['page'],
  components: {
    EventCard
  },
  data() {
    return {
      events: null,
      totalEvents : 0,
      totalPages : 0,
    }
  },
  created() {
    watchEffect(() => {
      this.events = null
      EventService.getEvents(2, this.page)
      .then(response => {
        this.events = response.data
        this.totalEvents = response.headers['x-total-count']
        this.totalPages = Math.ceil(this.totalEvents / 2)

      })
      .catch(error => {
        console.log(error)
      })
    })
  },
  computed: {
    hasNextPage() {
      return this.page < this.totalPages
    },
  }
}
</script>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
