<template>
  <main class="main-page">
    <article class="setting">
      <p>Отображение колонок:</p>
      <label class="label-checkbox">
        <input class="label-checkbox__input" type="checkbox" v-model="price">
        Список стоимостей
      </label>
      <label class="label-checkbox">
        <input class="label-checkbox__input" type="checkbox" v-model="story">
        Список истории курса конвертации
      </label>
    </article>
    <div class="main-page__container">

      <section class="base-block">
        <h3 class="base-block__title">Конвертация валют</h3>
        <div class="base-block__field">
          <div class="country-choose">
            <select class="country-choose__select" @change="updateCountry" v-model="countrySelectFirst">
              <option v-for="country in countries" :key="country">
                <p>{{country}}</p>
              </option>
            </select>
            <input class="country-choose__field" @keyup="updateInputs(event, 'first')" v-model="firstVal" />
          </div>
          <button class="separator-button" title="Переключить валюту" @click="switchCurrency">
            <img src="../assets/img/transfer-arrows.svg" />
          </button>
          <div class="country-choose">
            <select class="country-choose__select" v-model="countrySelectSecond">
              <option v-for="country in countries" :key="country">
                <p>{{country}}</p>
              </option>
            </select>
            <input class="country-choose__field" v-model="secondVal" @keyup="updateInputs(event, 'second')"/>
          </div>
        </div>
      </section>

      <section class="base-block" v-if="price">
        <h3 class="base-block__title">Список стоимостей</h3>
        
        <base-table :data="priceTable.data" :columns="priceTable.columns">
        </base-table>

      </section>
      <section class="base-block" v-if="story">

        <h3 class="base-block__title">Список истории курса конвертации</h3>
        <p>Отображение кофициента конвертации за последние 10 дней - USD, RUS, EUR</p>
      </section>

    </div>
  </main>

</template>

<script>
import axios from 'axios';
import BaseTable from '../components/BaseTable.vue';
import { baseUrl, apiKey } from '../config.js';

  export default {
    components: {
      BaseTable,
    },
    data() {
      return {
        firstVal: 0,
        secondVal: 0,
        dateStop: '2017-06-01',
        dateStart: '2017-06-01',
        countrySelectFirst: '',
        countrySelectSecond: '',
        price: false,
        story: true,
        countries: [
          'USA',
          'RUS',
        ],
        historyData: {
          data: []
        },
        priceTable: {
          columns: [{
            name: 'Стоимость',
            value: 'price'
          },
          {
            name: 'USD -> EUR',
            value: 'USD'
          },
          {
            name: 'EUR -> USD',
            value: 'EUR'
          }],
          data: [
            {
              price: 1,
              USD: '345',
              EUR: '123',
            }, 
            {
              price: 10,
              USD: '533',
              EUR: '433',
            }
          ]
        },
      };
    },
    methods: {
      updateInputs(event, input) {
        this.calculate(event, input);
      },

      calculate() {

      },

      getHiddenColumns() {
        this.price = localStorage.getItem('price') === 'true' ? true : false;
        this.story = localStorage.getItem('story') === 'true' ? true : false;
      },

      switchCurrency() {
        const tempCountry = this.countrySelectFirst;
        const tempVal = this.firstVal;
        this.countrySelectFirst = this.countrySelectSecond;
        this.countrySelectSecond = tempCountry;

        this.firstVal = this.secondVal;
        this.secondVal = tempVal;
      },

      getHistoryData() {
        this.historyData.data = this.action('convert?q=USD_RUB,RUB_USD&compact=ultra&');
      },

      getPriceTableData() {
        this.priceTable.data = this.action('convert?q=USD_RUB,RUB_USD&compact=ultra&');
      },

      getCountries() {
        this.countries = this.action('countries?');
      },

      async action(url) {
        const result = await axios.get(`${baseUrl}${url}apiKey=${apiKey}`);
        if (result.status === 200) {
          return result.data;
        }
        return null;
      }
    },
    computed: {
    },
    async mounted() {
      // /api/v7/countries?apiKey=[YOUR_API_KEY]
      // список стран

      // /api/v7/convert?q=USD_PHP,PHP_USD&compact=ultra&date=[yyyy-mm-dd]&endDate=[yyyy-mm-dd]&apiKey=[YOUR_API_KEY]
      // получение истории

      // const result = await axios.get('https://free.currconv.com/api/v7/convert?q=USD_RUB,RUB_USD&compact=ultra&apiKey=f7d2eaf94a143d47af5a');
      // let data = null;
      // if (result.status === 200) {
      //   data = result.data;
      // }

      this.getHiddenColumns();
    },
    watch: {
      price: function (val) {
        localStorage.setItem('price', val)
      },
      story: function (val) {
        localStorage.setItem('story', val)
      },
    },
  };
</script>

<style lang="scss">
body {
  font-size: 14px;
  margin: 0;
}

button {
  background-color: transparent;
  border: none;

  &:hover {
    opacity: .6;
  }
}

.main-page {
  display: flex;
  flex-direction: column;
  height: calc(100vh - 188px);
  padding: 30px;
  background-color: rgb(245, 245, 245);

  &__container {
    display: flex;
  }
}

.base-block {
  width: 30%;
  padding: 40px 20px;
  background-color: #fff;
  border-radius: 10px;

  &:not(:last-child) {
    margin-right: 2%;
  }

  &__title {
    font-size: 1.3rem;
    margin-top: 0;
  }

  &__field {
    display: flex;
  }
}

.country-choose {
  &__select {
    border-radius: 5px;
    padding: 6px 10px;
  }

  &__field {
    padding: 6px 10px;
    border: 1px solid #bebebe;
    border-radius: 5px;
    margin-top: 10px;
  }
}

.separator-button {
  cursor: pointer;

  img {
    width: 20px;
    height: 20px;
  }
}

.label-checkbox {
  display: flex;
  font-weight: 700;
  align-items: center;
  margin-bottom: 5px;

  &__input {
    margin-right: 10px;
  }
}

.setting {
  background-color: #f0f0f0;
  border: 1px solid rgba(138, 138, 138, 0.411);
  padding: 20px;
  width: auto;
  margin-bottom: 10px;
  border-radius: 5px;
}
</style>