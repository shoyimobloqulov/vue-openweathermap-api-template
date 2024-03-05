<script>
  import axios from 'axios';
  import moment from 'moment-timezone';


  export default{
    data(){
      return {
        city: '',
        weatherData: '',
        imgSrc:'',
        error: '',
        showB: false
      }
    },
    methods: {
      getData(){
        this.showB = true
        // https://api.openweathermap.org/data/2.5/weather?q=CITY_NAME&appid=YOUR_API_KEY

        // API key
        const apiKey = '4e1287adcd7c5245fbe10f63a2ca8fd2';
        
        // Api request city
        const city = this.city;
        
        // Axios Requets
        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}`)
        .then(response => {
          this.showB = false
          this.error = ''
          this.weatherData = response.data;
          let iconCode = response.data.weather[0].icon
          this.imgSrc = `http://openweathermap.org/img/w/${iconCode}.png`;
        })
        .catch(error => {
          this.weatherData = ''
          this.error = "Ma'lumotlar topilmadi, qayta urinib ko'ring"
        });
      }
    }
  }
</script>

<template>
  <div class="flex justify-center mt-5">
    <a href="#" class="block max-w-md p-6 bg-white border border-gray-200 rounded-lg shadow hover:bg-gray-100 dark:bg-gray-800 dark:border-gray-700 dark:hover:bg-gray-700">
      <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">Ob-havo malumotlari</h5>
      <div class="font-normal text-gray-700 dark:text-gray-400">
        <div>
            <label for="first_name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Kiritmoqchi bo'lgan shaharingiz {{ city != "" ? city : "Samarkand" }}</label>
            <input v-model="city"  type="text" id="first_name" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required />
        </div>
        <button v-show="city != ''" type="submit" @click="getData" class="mt-3 text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"><i class="fa fa-search"></i> Tekshirish</button>
      </div>
      <br>
      <p v-if="error != ''" class="text-red-500">{{ error }}</p>
      <div v-if="showB" class="flex content-center">
        <div class="animate-spin rounded-full h-16 w-16 border-t-4 border-blue-500"></div>
      </div>
      <div v-if="weatherData != ''">
        <p class="bg-green-600 border rounded-full w-14 text-center text-white">{{ weatherData.cod }}</p>
        <div class="grid grid-cols-3 gap-4">
          <div>
            <img v-bind:src="imgSrc" class="w-25 h-25 text-gray-500 dark:text-gray-400 mb-3" alt="">
          </div>
          <div>
            <h2>{{ weatherData.weather[0].main }}</h2>
            <p>{{ weatherData.weather[0].description }}</p>
          </div>
        </div>
        
        <a href="#">
            <h5 class="mb-2 text-2xl font-semibold tracking-tight text-gray-900 dark:text-white">{{ weatherData.sys.country }} / {{ weatherData.name }} / [{{ weatherData.coord.lon }} , {{ weatherData.coord.lat }}]</h5>
        </a>
        

        <div class="relative">
            <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
                <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
                    <tr>
                        <th scope="col" class="px-6 py-3">
                            Tempratura
                        </th>
                        <th scope="col" class="px-3 py-3">
                            {{ weatherData.main.temp }} / {{ weatherData.main.feels_like }}
                        </th>
                    </tr>

                    <tr>
                        <th scope="col" class="px-6 py-3">
                            Max/Min Tempratura
                        </th>
                        <th scope="col" class="px-3 py-3">
                            {{ weatherData.main.temp_min }} / {{ weatherData.main.temp_max }}
                        </th>
                    </tr>
                </thead>
                <tbody>
                  
                </tbody>
            </table>
        </div>

      </div>
    </a>
  </div>

</template>

<style scoped>
  
</style>
