<template>
  <div class="m-2 mx-lg-5 d-flex justify-content-end">
      <div @click="toggleUnit" class="unitToggle p-3 py-2 rounded-pill" :class="[results.unit ? 'bg-dark text-light' : 'bg-light text-dark']">
        {{results.unit ? "Metric °C" : "Imperial °F"}}
      </div>
  </div>
  <div class="card mx-lg-5">
    <div class="row mt-3 mt-md-0">
      <div class="col p-sm-3">
        <h4 class="ms-4">The Weather in <span class="bg-dark text-light py-1 px-2 rounded">{{data.location.name}}</span></h4>
        <h4 class="ms-4 text-secondary fs-5">{{data.location.localtime}}</h4>
      </div>
      <div class="col d-flex flex-column flex-sm-row align-items-center justify-content-center">
        <img :src="data.current.condition.icon" :title="data.current.condition.text" :alt="data.current.condition.text">
        <h4 class="font-weight-light"><span class="fs-4 text-secondary">{{data.current.condition.text}} </span> {{results.unit ? `${data.current.feelslike_c}°C` : `${data.current.feelslike_f}°F`}} <span class="fs-6 text-secondary">RealFeel</span></h4>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <ul class="list-group list-group-flush p-4">
          <li class="list-group-item d-flex align-items-center p-0 py-2">
            <p class="col d-flex align-items-center fs-5 m-0"><i class="las la-thermometer-three-quarters fs-1"></i> Temperature</p>
            <p class="align-self-end fs-5 m-0">{{results.unit ? `${data.current.temp_c}°C`:`${data.current.temp_f}°F`}}</p>
          </li>
          <li class="list-group-item d-flex align-items-center p-0 py-2">
            <p class="col d-flex align-items-center fs-5 m-0"><i class="las la-tint fs-1"></i> Humidity</p>
            <p class="align-self-end fs-5 m-0">{{data.current.humidity}}%</p>
          </li>
          <li class="list-group-item d-flex align-items-center p-0 py-2">
            <p class="col d-flex align-items-center fs-5 m-0"><i class="las la-compress fs-1"></i> Pressure</p>
            <p class="align-self-end fs-5 m-0">{{results.unit?`${data.current.pressure_mb}mBar`:`${data.current.pressure_in}PSI`}}</p>
          </li>
          <li class="list-group-item d-flex align-items-center p-0 py-2">
            <p class="col d-flex align-items-center fs-5 m-0"><i class="las la-eye fs-1"></i> Visibility</p>
            <p class="align-self-end fs-5 m-0">{{results.unit?`${data.current.vis_km} Km`:`${data.current.vis_miles} Miles`}}</p>
          </li>
          <li class="list-group-item d-flex align-items-center p-0 py-2">
            <p class="col d-flex align-items-center fs-5 m-0"><i class="las la-cloud-rain fs-1"></i> Precipitation</p>
            <p class="align-self-end fs-5 m-0">{{results.unit?`${data.current.precip_mm} mm`: `${data.current.precip_in} in`}}</p>
          </li>
        </ul>
      </div>
      <div class="col-md">
        <ul class="list-group list-group-flush p-4">
          <li class="list-group-item d-flex align-items-center p-0 py-2">
            <p class="col d-flex align-items-center fs-5 m-0"><i class="las la-wind fs-1"></i> Wind</p>
            <p class="align-self-end fs-5 m-0">{{results.unit?`${data.current.wind_kph} Km/h`:`${data.current.wind_mph} Mi/h`}} | {{data.current.wind_degree + ' ' + data.current.wind_dir}}</p>
          </li>
          <li class="list-group-item d-flex align-items-center p-0 py-2">
            <p class="col d-flex align-items-center fs-5 m-0"><i class="las la-sun fs-1"></i> Uv Index</p>
            <p class="align-self-end fs-5 m-0">{{data.current.uv}} of 10</p>
          </li>
          <li class="list-group-item d-flex align-items-center p-0 py-2">
            <p class="col d-flex align-items-center fs-5 m-0"><i class="las la-moon fs-1"></i> Moon Phase</p>
            <p class="align-self-end fs-5 m-0">{{data.forecast.forecastday[0].astro.moon_phase}}</p>
          </li>
          <li class="list-group-item d-flex align-items-center p-0 py-2">
            <p class="col d-flex align-items-center fs-5 m-0"><i class="lab la-envira fs-1"></i> Air Quality</p>
            <p class="align-self-end fs-5 m-0">{{data.current.air_quality['us-epa-index']}}</p>
          </li>
          <li class="list-group-item d-flex align-items-center p-0 py-2">
            <p class="col d-flex align-items-center fs-5 m-0"><i class="las la-sun fs-1"></i> <i class="las la-moon fs-1"></i> Sunrise/Sunset</p>
            <p class="align-self-end fs-5 m-0">{{data.forecast.forecastday[0].astro.sunrise + ' / ' + data.forecast.forecastday[0].astro.sunset}}</p>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'HeroCard',
    props: {
      data: Object,
      isMetric: Boolean
    },
    data() {
      return {
        results: {
          unit: this.isMetric,
        }
      }
    },
    methods: {
      toggleUnit() {
        this.results.unit = !this.results.unit;
      }
    },
    watch: {
      isMetric() {
        this.results.unit = this.isMetric;
      }
    }
  }
</script>

<style scoped>
.unitToggle {
  cursor: pointer;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  transition: 0.3s;
}
</style>