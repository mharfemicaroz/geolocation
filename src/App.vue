<template>
  {{ jsonData }}
</template>

<script>
export default {
  data() {
    return {
      jsonData: {},
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
      const par = null;
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(this.displayData);
      } else {
        this.jsonData = "Geolocation is not supported by this browser.";
      }
    },
    displayData(position) {
      this.jsonData = {
        timestamp: new Date().toISOString(),
        coordinates: {
          latitude: position.coords.latitude,
          longitude: position.coords.longitude,
        },
      };
    },
  },
};
</script>

<style scoped>
/* Add any styling here */
</style>
