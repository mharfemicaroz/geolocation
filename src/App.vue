<template>
  {{ lastCoordinate }}
  <div>
    <iframe
      width="800"
      height="600"
      frameborder="0"
      style="border: 0"
      referrerpolicy="no-referrer-when-downgrade"
      :src="`https://www.google.com/maps/embed/v1/place?key=AIzaSyCMJwrVEJhnB8KqT7zQUV0w2sMIsEMO8NM&q=${lastCoordinate.latitude},${lastCoordinate.longitude}`"
      allowfullscreen
    >
    </iframe>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      jsonData: {},
      lastCoordinate: { latitude: null, longitude: null },
      options: {
        enableHighAccuracy: true,
        timeout: 5000,
        maximumAge: 10000,
      },
    };
  },
  created() {
    this.generateData();
  },
  mounted() {},
  beforeDestroy() {},
  methods: {
    generateData() {
      if (navigator.geolocation) {
        navigator.geolocation.watchPosition((position) => {
          this.displayData(position.coords, position.timestamp);
          axios
            .post("https://sumharfe.pythonanywhere.com/location/", {
              timestamp: position.timestamp,
              latitude: position.coords.latitude,
              longitude: position.coords.longitude,
            })
            .then((res) => {
              axios
                .get("https://sumharfe.pythonanywhere.com/location/")
                .then((response) => {
                  this.lastCoordinate = response.data[response.data.length - 1];
                });
            });
        });
      } else {
        this.jsonData = "Geolocation is not supported by this browser.";
      }
    },
    displayData(position, timestamp) {
      this.jsonData = {
        timestamp: new Date(timestamp),
        coordinates: {
          latitude: position.latitude,
          longitude: position.longitude,
          accuracy: position.accuracy,
        },
        version: "2.5.0",
      };
    },
    displayError(err) {
      console.warn(`ERROR(${err.code}): ${err.message}`);
    },
  },
};
</script>

<style scoped>
/* Add any styling here */
</style>
