<template>
  <div id="app">
    <sightings-form />
    <sightings-grid :sightings="sightings" />
  </div>
</template>

<script>
import{ eventBus } from '@/main.js'
import SightingService from '@/services/SightingService.js'
import SightingsForm from './components/SightingsForm';
import SightingsGrid from './components/SightingsGrid';


export default {
  name: 'app',
  components: {
    'sightings-form': SightingsForm,
    'sightings-grid': SightingsGrid
  },

  data() {
    return {
      sightings: []
    };
  },

	mounted() {
    this.fetchSightings();

    eventBus.$on('bird-added', (bird) => {
      this.sightings.push(bird)
    })

    eventBus.$on('bird-deleted', (id) => {
      let index = this.sightings.findIndex(bird => bird._id === id)
      this.sightings.splice(index, 1)
    })
  },

  methods: {
    fetchSightings() {
      SightingService.getSightings()
        .then(sightings => this.sightings = sightings);
    }
  }
}
</script>

<style>
html {
  height: 100%;
}

body {
  background: url('./assets/birds-background.jpg') no-repeat;
  height: 100%;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;

}
</style>
