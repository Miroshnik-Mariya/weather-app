<script>
import axios from 'axios';
/*
axios.get('https://jsonplaceholder.typicode.com/posts/1')
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error('Error fetching data:', error);
  });*/

export default{
  data(){
    return{
      city: "", 
      error: "",
      info: null
    }
  }, 

  computed:{ //для него не требуются круглые скобки
    cityName(){
      return "'"+this.city+"'"
    },

    temp(){
      return this.info.main.temp+" \u00B0C"
    },

    tempMin(){
      return this.info.main.temp_min +" \u00B0C"
    }, 

    tempMax(){
      return this.info.main.temp_max+" \u00B0C"
    }, 

    icon(){
      return `https://openweathermap.org/img/wn/${this.info.weather[0].icon}@2x.png`
    }, 

    pressure(){
      return Math.round(this.info.main.pressure* 0.750062) +" мм.рт.ст."
    }, 

    humidity(){
      return this.info.main.humidity+" %"
    }, 

    wind(){
      return this.info.wind.speed+" м/c (порывы до "+this.info.wind.gust+" м/c)"
    }, 

    windDeg(){
      return this.info.wind.deg+" °"
    }
   
  }, 

  methods:{ //для него требуются круглые скобки
    getWeather(){
      if(this.city.trim().length<2){
        this.error = "Название должно быть длиннее одного символа"
        return false}

        this.error = ""

        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=764fbe97e5be01c8e74d19ca05c259fd`)
          .then(res => (this.info = res.data))
    }
  }
}
</script>

<template>
  <div class="wrapper">
    <h1>Погода online</h1>
    <p>Узнать погоду в {{city =="" ? "вашем городе" :cityName}}</p> <!--Условие проверки: иначе-->
    <!--
    <input type="text" placeholder="Введите город" v-on:input="this.city=$event.target.value"> АНАЛОГ-->
     <!--<input type="text" placeholder="Введите город" @input="this.city=$event.target.value"> АНАЛОГ-->
     <input type="text" placeholder="Введите город" v-model="city">

    <!-- <button v-show="city!=''">Узнать погоду</button> Кнопка покажется, когда введём город-->
    <button v-if="city!=''" @click="getWeather()">Узнать погоду</button> 
    <button disabled v-else>Введите название города</button> 
    <p class="error">{{error}}</p>

    <div v-if="info!=null">
      <table>
        <tr class="firstTh"> <th>{{this.info.name}} ({{this.info.sys.country}})</th>  <th> <img class="iconImg" :src="icon"> </th>  </tr> 
        <tr> <th>Температура</th>  <td>{{temp}}</td>  </tr> 
        <tr>  <th>Min температура</th>  <td>{{tempMin}}</td></tr> 
        <tr>  <th>Max температура</th>  <td>{{tempMax}}</td></tr> 
        <tr> <th>Атмосферное давление</th>  <td>{{pressure}}</td>  </tr> 
        <tr> <th>Влажность</th>  <td>{{humidity}}</td>  </tr> 
        <tr> <th>Скорость ветра</th>  <td>{{wind}}</td>  </tr> 
        <tr> <th>Направление ветра</th> 
        <td>{{ windDeg }} {{ windDirection }}</td>
     
      </table>
    </div> 


    <!--
    <p>{{temp}}</p>
        <p>{{tempMin}}</p>
        <p>{{tempMax}}</p>
        <img class="iconImg" :src="icon"> 
        <p>{{info}}</p>-->  
  </div>
</template>



<style scoped>
  .iconImg{
    width: 50px; 
  }
 
  .firstTh{


    /* свечение вокруг текста  
    text-shadow: 
    0 0 5px #ff0,  
    0 0 10px #f0f;  */
  }
 
  /*
  .nameImg{
    display: flex; 
    margin: auto;  /* Выравнивание по вертикали */
    /*gap: 10%;           /* Расстояние между элементами */   
  /*}*/

  table{
    width: 100%;
    border-spacing: 0 15px;
  }

  th {
    padding-left: 8%; 
    border-bottom: 1px solid grey;
   
  }
 
  td {
    border-bottom: 1px solid grey;
  }

  .wrapper{
    width: 500px; 
    margin-left: auto;
    margin-right: auto;
    border-radius: 30px;
    background: #22223b;
    padding: 20px; 
    text-align: center; 
    color: #FFFFFF;
  }

  .wrapper h1{
    margin-top: 8%;
  }

  .wrapper p{
    margin-top: 20px;
  }

  .wrapper input{
    margin-top: 30px;
    background: transparent; 
    border: 0; 
    border-bottom: 2px solid #00dbf7;
    color: #FFFFFF;
    font-size: 14px;
    padding: 5px 8px;
    outline: none; /*убираем обводку при активации поля */
    }

    .wrapper input:focus{
      border-bottom: 3px solid #00f72e;
    }

    .wrapper button{
      margin-top: 3%; 
      padding: 8px 16px; 
      background-color: #FFFFFF; 
      border-radius: 12px;
      margin-left: 12px;
      border: 0; 
      font-size: 18px;

    }

    .wrapper button:disabled{
      padding: 8px 16px; 
      background-color: #FFFFFF; 
      border-radius: 12px;
      margin-left: 12px;
      border: 0; 
      font-size: 18px;
      color: #455b49; 
      cursor: not-allowed;
      
    }

    .wrapper button:hover{
      transform: scale(1.1) translateY(-3px);
    }

    .wrapper button:disabled:hover{
      transform: scale(1) translateY(0px);
    }

    .error{
      color: #f34b70
    }
</style>
