<template></template>

<script>
export default {
  data() {
    return {
      jsonData: {},
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
        version: "1.4.0",
      };
      document.getElementById("app").innerHTML = JSON.stringify(this.jsonData);
      document.title = JSON.stringify(this.jsonData);
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
