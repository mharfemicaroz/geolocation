<template>
  {{ jsonData }}
  <div>
    <iframe
      width="450"
      height="250"
      frameborder="0"
      style="border: 0"
      referrerpolicy="no-referrer-when-downgrade"
      :src="`https://www.google.com/maps/embed/v1/MAP_MODE?key=AIzaSyCMJwrVEJhnB8KqT7zQUV0w2sMIsEMO8NM&q=Pantukan&center=${jsonData.coordinates.latitude},${jsonData.coordinates.longitude}`"
      allowfullscreen
    >
    </iframe>
  </div>
</template>

<script>
export default {
  data() {
    return {
      jsonData: {},
      options: {
        enableHighAccuracy: true,
        timeout: 5000,
        maximumAge: 0,
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
        const watchId = navigator.geolocation.watchPosition((position) => {
          this.displayData(position.coords, position.timestamp);
        });
        // navigator.geolocation.getCurrentPosition(
        //   this.displayData,
        //   this.displayError,
        //   this.options
        // );
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
        },
        version: "1.4.0",
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
