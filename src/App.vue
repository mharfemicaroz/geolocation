<template>
  {{ jsonData }}
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
  mounted() {
    this.generateDataInterval = setInterval(this.generateData, 1000);
  },
  beforeDestroy() {
    clearInterval(this.generateDataInterval);
  },
  methods: {
    generateData() {
      if (navigator.geolocation) {
        navigator.geolocation.watchPosition((position) => {
          this.displayData(position.coords);
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
    displayData(position) {
      this.jsonData = {
        timestamp: new Date().toISOString(),
        coordinates: {
          latitude: position.latitude,
          longitude: position.longitude,
        },
        version: "1.3.0",
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
