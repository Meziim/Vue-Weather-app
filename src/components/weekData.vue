<template>
  <div class="card col-md-9">
    <div class="parent-card card-body d-flex">
      <div 
        :key="forecastDays.indexOf(day)" 
        v-for="day in forecastDays" 
        class="card flex-fill p-1 m-1"
      >
        <div class="small-card card-body d-flex flex-column">
          <h4 class="text-center fs-6">{{day.date}}</h4>
          <h4 class="text-center">{{day.day.condition.text}}</h4>
          <img class="card-thumbnail mx-auto" :src="day.day.condition.icon" :alt="day.day.condition.text">
          <h4 class="text-center"></h4>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'weekData',
  components: {

  },
  props: {
    forecastDays: Object,
  }

}
</script>

<style scoped>
  .parent-card {
    height: 300px;
    
  }
  .small-card {
    justify-content: space-between;
  }
  .card .card-thumbnail {
    width: 100px;

  }
</style>