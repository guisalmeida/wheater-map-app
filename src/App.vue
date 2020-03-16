<template>
  <div class="container">
    <div class="loading" v-if="isLoading">
      <div class="loading-spinner"></div>
    </div>

    <h1 class="title">Weather App <span>made with Vue.js</span></h1>
    <div class="container-content">
  
      <transition name="appear">
        <div class="content-temp" v-if="temp.current">
          <span class="content-temp-current">{{ temp.current }} ºC</span>
          <div class="content-temp-minmax">
            <p>Min: {{ temp.min }} ºC</p>
            <p>Max: {{ temp.max }} ºC</p>
          </div>
        </div>
      </transition>

      <button class="btn" v-on:click="loadWeather">Load Weather</button>
    </div>
  </div>
</template>

<script>
  const API_KEY = '0d17a64705c428f1d87a60d17fc424a0'
  const API_URI = `https://api.openweathermap.org/data/2.5/weather`
  const mountCoords = (navigator) => {
    return new Promise(resolve => {
      navigator.geolocation.getCurrentPosition(position => {
        const { latitude, longitude } = position.coords; 
        resolve(`?lat=${latitude}&lon=${longitude}&appid=${API_KEY}&units=metric`)
      })

    })
  }

  export default {
    name: 'WeatherApp',
    data:() => ({
      isLoading: false,
      temp: {
        current: 0,
        max: 0,
        min: 0
      }
    }),
    methods: {
      loadWeather() {
        this.isLoading = true;
        mountCoords(window.navigator)
          .then(query => API_URI+query)
          .then(url => fetch(url))
          .then(res => res.json())
          .then(json => {
            this.temp.current = json.main.temp.toFixed(0);
            this.temp.max = json.main.temp_max.toFixed(0);
            this.temp.min = json.main.temp_min.toFixed(0);
            this.isLoading = false;
          })
      }
    }
  }
</script>

<style>
* { margin: 0; padding: 0; box-sizing: border-box; outline: none; }
html,
body { width: 100%; height: 100%; font-family: Helvetica }
.appear-enter-active, .appear-leave-active {
  transition: all .9s cubic-bezier(0.17, 1.03, 0.23, 1.25);
}
.appear-enter, .appear-leave-to {
  transform: translateY(-10px);
}
@keyframes spin {
  to { transform: rotate(360deg); }
}
.loading {
  position: absolute;
  z-index: 9999;
  width: 100%;
  height: 100%;
  display: flex;
  background-color: rgba(0, 0, 0, 0.8);
  justify-content: center;
  align-items: center;
}
.loading-spinner {
  border: 8px solid rgb(255, 255, 255);
  border-radius: 50%;
  border-left-color: lightcoral;
  width: 50px;
  height: 50px;
  animation: spin .2s linear infinite;
}
.container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #f4f4f4;
}
.container-content {
  width: 320px;
  margin-top: 50px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.content-temp {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 25px;
}
.content-temp-current {
  font-size: 30px;
  color: teal;
  font-weight: 700;
}
.content-temp-minmax {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  font-size: 20px;
  color: #ccc;
  font-weight: 300;
}
.title {
  position: relative;
  color: lightcoral;
  font-size: 50px;
}
.title > span {
  position: absolute;
  font-size: 15px;
  right: -50px;
  transform: rotate(-10deg);
  background-color: #444;
  color: #fff;
  padding: 5px 10px;
  border-radius: 4px;
  font-weight: 100;
}
.container-content .btn {
  border: none;
  color: white;
  padding: 10px;
  border-radius: 5px;
  cursor: pointer;
  text-transform: uppercase;
  background-color: teal;
  font-weight: 700;
  transition: all .3s ease-in;
}
.container-content .btn:hover {
  box-shadow: 1px 1px 30px 4px #ccc;
}
</style>

