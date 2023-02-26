<template>
    <div>
      <div id="map" class="tem-box" />
      {{ place }}??
      <ul>
        <li v-for="(location, index) in locations" :key="location.id">
          {{ index }}. {{ location.name }}
          <a href="#" class="btn btn-info"
          @click.prevent="mapMove(location.latLng)">TESTT</a>
        </li>
      </ul>
    </div>
</template>

<style>
  .tem-box {
    height: 300px;
    display: flex;
    align-items: center;
  }
</style>

<script>
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';

const url = import.meta.env.VITE_URL;

export default {
  data() {
    return {
      place: {
        x: 51.505,
        y: -0.09,
      },
      locations: [],
      map: {},
    };
  },
  created() {
    this.$http.get(`${url}locations?_expand=county&_expand=category`)
      .then((res) => {
        this.locations = res.data;
      })
      .catch((err) => {
        console.error(err);
      });
  },
  mounted() {
    this.map = L.map('map', {
      center: [this.place.x, this.place.y],
      zoom: 13,
    });
    L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 19,
      attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
    }).addTo(this.map);
    L.marker([this.place.x, this.place.y]).addTo(this.map);
    console.log('testEnv - url', url);
  },
  methods: {
    mapMove(latLng) {
      [this.place.x, this.place.y] = latLng;
      this.map.panTo([this.place.x, this.place.y]);
    },
  },
};
</script>
