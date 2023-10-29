<script>
// @ is an alias to /src
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService.js'

const perPage = 2

export default {
  name: 'EventList',
  props: ['page'],
  components: {
    EventCard
  },
  data() {
    return {
      events: null,
      totalEvents: 0
    }
  },
  beforeRouteEnter(routeTo, routeFrom, next) {
    EventService.getEvents(perPage, parseInt(routeTo.query.page) || 1)
      .then((res) => {
        next((comp) => {
          comp.events = res.data
          comp.totalEvents = res.headers['x-total-count']
        })
      })
      .catch(() => {
        next({ name: 'NetworkError' })
      })
  },
  beforeRouteUpdate(routeTo) {
    return EventService.getEvents(perPage, parseInt(routeTo.query.page) || 1)
      .then((res) => {
        this.events = res.data
        this.totalEvents = res.headers['x-total-count']
      })
      .catch(() => {
        return { name: 'NetworkError' }
      })
  },
  computed: {
    hasNextPage() {
      const totalPages = Math.ceil(this.totalEvents / perPage)
      return this.page < totalPages
    }
  }
}
</script>

<template>
  <h1>Events For Good</h1>
  <div class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event" />

    <div class="pagination">
      <router-link
        v-if="page != 1"
        id="page-prev"
        :to="{ name: 'EventList', query: { page: page - 1 } }"
        rel="prev"
        >&lt; Previous</router-link
      >

      <router-link
        v-if="hasNextPage"
        id="page-next"
        :to="{ name: 'EventList', query: { page: page + 1 } }"
        rel="next"
        >Next &gt;</router-link
      >
    </div>
  </div>
</template>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 18px;
}

.pagination {
  width: 290px;
  position: relative;
}

.pagination a {
  color: #2c3e50;
  text-decoration: none;
}

#page-prev {
  position: absolute;
  left: 0;
}

#page-next {
  position: absolute;
  right: 0;
}
</style>
