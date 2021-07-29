<template>
  <main class="main-page">
    <div class="main-page__container">
      <section class="base-block">
        <h3 class="base-block__title">Конвертация валют</h3>
        <div class="base-block__field">
          <div class="country-choose">
            <select class="country-choose__select" @change="updateInputs" v-model="countrySelectFirst">
              <option v-for="country in countries" :key="country">
                <p>{{country}}</p>
              </option>
            </select>
            <input class="country-choose__field" v-model="firstVal" />
          </div>
          <button class="separator-button" title="Переключить валюту">
            <img src="../assets/img/transfer-arrows.svg" />
          </button>
          <div class="country-choose">
            <select class="country-choose__select" @change="updateInputs" v-model="countrySelectSecond">
              <option v-for="country in countries" :key="country">
                <p>{{country}}</p>
              </option>
            </select>
            <input class="country-choose__field" v-model="firstVal" />
          </div>
        </div>
      </section>

      <section class="base-block">
        <h3 class="base-block__title">Список стоимостей</h3>
        
        <base-table :data="priceTable.data" :columns="priceTable.columns">
        </base-table>

      </section>
      <section class="base-block">

        <h3 class="base-block__title">Список истории курса конвертации</h3>

      </section>
    </div>
  </main>

</template>

<script>
import axios from 'axios';
import BaseTable from '../components/BaseTable.vue';

  export default {
    components: {
      BaseTable,
    },
    data() {
      return {
        firstVal: 0,
        secondVal: 0,
        countrySelectFirst: '',
        countrySelectSecond: '',
        countries: [
          'USA',
          'RUS',
        ],
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
      updateInputs() {

      }
    },
    async mounted() {
      const result = await axios.get('https://free.currconv.com/api/v7/convert?q=USD_RUB,RUB_USD&compact=ultra&apiKey=f7d2eaf94a143d47af5a');
      let data = null;
      if (result.status === 200) {
        data = result.data;
      }
      console.log('result', result);
      console.log('data', data);
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
  min-height: calc(100vh - 128px);
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
</style>