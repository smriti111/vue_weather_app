<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
    <div class="search-box">
      <input 
      type="text"
      class="search-bar"
      placeholder="Search..."
      v-model="query"
      @keyup.enter="weather_fetch">
    </div>
    <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
      <div class="location-box">
        <div class="location">
        {{weather.name}} {{weather.sys.country}}
        <div class="date">
          <p>{{date_today()}}</p>
        </div>
      </div>
      </div>
      
    </div>

    <div class="weather-box">
      <div class="temp">
        {{ Math.round(temp)}}°c
        <div class="weather">
          {{weather_type}}
        </div>
      </div>
    </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data(){
    return{
      api_key:"473f273f1c212ccc98b35b1b1580d3c6",
      query:"",
      url:"https://api.openweathermap.org/data/2.5/",
      weather:[],
      temp:"",
      weather_type:"",
    }
  },

  methods:{
    Search(query){
      console.log(query)
    },
    weather_fetch(){
      fetch(`${this.url}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
      .then(res => {
            return res.json();
          }).then(this.setResults);
    },
    setResults(results){
      //console.log(results.name)
      this.weather=results,
      this.temp=this.weather.main.temp,
      this.weather_type=this.weather.weather[0].main,
      console.log(this.weather.main.temp)
      console.log(this.weather.weather[0].main)

    },

    date_today(){
      let d= new Date()
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day= days[d.getDay()]
      let date= d.getDate()
      let month= months[d.getMonth()]
      let year= d.getFullYear()
      console.log(day,date,month)
      return `${date} ${day} , ${month},${year}`
    }

  }
}



</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'montserrat', sans-serif;
}
#app {
  background-image: url('./assets/cold-bg.jpg'); 
  background-size: contain;
  background-position: bottom; 
  transition: 0.4s;
} 

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
} 
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  
  color: #313131;
  font-size: 20px;
  appearance: none;
  border:none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}


.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
