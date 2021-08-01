<template>
  <main class="main-page">
    <article class="setting">
      <p>Отображение колонок:</p>
      <label class="label-checkbox">
        <input class="label-checkbox__input" type="checkbox" v-model="price" />
        Список стоимостей
      </label>
      <label class="label-checkbox">
        <input class="label-checkbox__input" type="checkbox" v-model="story" />
        Список истории курса конвертации
      </label>
    </article>
    <div class="main-page__container">
      <section class="base-block">
        <h3 class="base-block__title">Конвертация валют</h3>
        <div class="base-block__field">
          <div class="country-choose">
            <select
              class="country-choose__select"
              @change="updateCountry"
              v-model="countrySelectFirst"
            >
              <option v-for="country in countries" :key="country">
                <p>{{ country }}</p>
              </option>
            </select>
            <input
              class="country-choose__field"
              @keyup="calculate($event, 'first')"
              v-model.number="firstVal"
            />
          </div>
          <button
            class="separator-button"
            title="Переключить валюту"
            @click="switchCurrency"
          >
            <img src="../assets/img/transfer-arrows.svg" />
          </button>
          <div class="country-choose">
            <select
              class="country-choose__select"
              @change="updateCountry"
              v-model="countrySelectSecond"
            >
              <option v-for="(country, index) in countries" :key="index">
                <p>{{ country }}</p>
              </option>
            </select>
            <input
              class="country-choose__field"
              v-model.number="secondVal"
              @keyup="calculate($event, 'second')"
            />
          </div>
        </div>
      </section>

      <section class="base-block base-block--price" v-if="price">
        <h3 class="base-block__title">Список стоимостей</h3>

        <base-table :data="priceTable.data" :columns="priceTable.columns">
        </base-table>
      </section>
      <section class="base-block base-block--story" v-if="story">
        <h3 class="base-block__title">Список истории курса конвертации</h3>
        <p>
          Отображение кофициента конвертации за последние 10 дней - USD, RUS
        </p>

        <base-table :data="historyData.data" :columns="historyData.columns">
        </base-table>
      </section>
    </div>
  </main>
</template>

<script>
import axios from "axios";
import BaseTable from "../components/BaseTable.vue";
import api from "../config.js";

export default {
  components: {
    BaseTable,
  },
  data() {
    return {
      firstVal: null,
      secondVal: null,
      countrySelectFirst: "",
      countrySelectSecond: "",
      price: false,
      story: true,
      countries: [],
      firstCurrentUnit: 0,
      secondCurrentUnit: 0,
      historyData: {
        columns: [
          {
            name: "Дата",
            value: "date",
          },
          {
            name: "USD -> RUS",
            value: "USD",
          },
          {
            name: "RUS -> USD",
            value: "RUS",
          },
        ],
        data: [],
      },
      priceTable: {
        columns: [
          {
            name: "Стоимость",
            value: "price",
          },
          {
            name: "USD -> EUR",
            value: "USD",
          },
          {
            name: "EUR -> USD",
            value: "EUR",
          },
        ],
        data: [],
      },
    };
  },
  methods: {
    async updateCountry() {
      if (this.countrySelectFirst && this.countrySelectSecond) {
        const firstCurrency = `${this.countrySelectFirst}_${this.countrySelectSecond}`;
        const secondCurrency = `${this.countrySelectSecond}_${this.countrySelectFirst}`;
        console.log(firstCurrency);
        console.log(secondCurrency);
        //const result = await this.action(`convert?q=${firstCurrency},${secondCurrency}&compact=ultra&`);
        //this.firstCurrentUnit = result[firstCurrency];
        //this.secondCurrentUnit = result[secondCurrency];
        this.firstCurrentUnit = 4.358991;
        this.secondCurrentUnit = 14.358991;
      }
    },

    calculate(event, input) {
      if (input === "first") {
        this.secondVal = (this.firstVal * this.firstCurrentUnit).toFixed(2);
      } else {
        this.firstVal = (this.secondVal * this.secondCurrentUnit).toFixed(2);
      }
    },

    getHiddenColumns() {
      this.price = localStorage.getItem("price") === "true" ? true : false;
      this.story = localStorage.getItem("story") === "true" ? true : false;
    },

    switchCurrency() {
      const tempCountry = this.countrySelectFirst;
      const tempVal = this.firstVal;
      const tempUnit = this.firstCurrentUnit;
      this.countrySelectFirst = this.countrySelectSecond;
      this.countrySelectSecond = tempCountry;

      this.firstVal = this.secondVal;
      this.secondVal = tempVal;

      this.firstCurrentUnit = this.secondCurrentUnit;
      this.secondCurrentUnit = tempUnit;
    },

    async getHistoryData() {
      //let data = await this.action(`convert?q=USD_RUB,RUB_USD&compact=ultra&date=${this.dateTenDays}&endDate=${this.dateNow}&`);
      let data = {
        RUB_USD: {
          "2021-07-24": 0.013538,
          "2021-07-25": 0.013565,
          "2021-07-26": 0.013566,
          "2021-07-27": 0.013561,
          "2021-07-28": 0.013612,
          "2021-07-29": 0.013663,
          "2021-07-30": 0.013671,
          "2021-07-31": 0.013671,
          "2021-08-01": 0.013671,
        },
        USD_RUB: {
          "2021-07-24": 73.868204,
          "2021-07-25": 73.717304,
          "2021-07-26": 73.716099,
          "2021-07-27": 73.741298,
          "2021-07-28": 73.465198,
          "2021-07-29": 73.188012,
          "2021-07-30": 73.149404,
          "2021-07-31": 73.149404,
          " 2021-08-01": 73.149404,
        },
      };

      data.RUB_USD = Object.entries(data.RUB_USD);
      data.USD_RUB = Object.entries(data.USD_RUB);
      const keyLength = Object.keys(data.RUB_USD).length;

      for (let i = 0; i < keyLength; i += 1) {
        this.historyData.data.push({
          USD: data.USD_RUB[i][1],
          RUS: data.RUB_USD[i][1],
          date: data.RUB_USD[i][0],
        });
      }
    },

    async getPriceTableData() {
      //let data = await this.action('convert?q=USD_EUR,EUR_USD&compact=ultra&');
      const data = {
        EUR_USD: 1.1867,
        USD_EUR: 0.842673,
      };

      const unit = [1, 5, 10, 25, 50, 100, 500, 1000, 5000];

      for (let i = 0; i < unit.length; i += 1) {
        this.priceTable.data.push({
          price: unit[i],
          USD: (data.USD_EUR * unit[i]).toFixed(2),
          EUR: (data.EUR_USD * unit[i]).toFixed(2),
        });
      }
    },

    async getCountries() {
      //let data = await this.action('countries?');
      // data = data.results;
      let data = {
        AD: {
          alpha3: "AND",
          currencyId: "EUR",
          currencyName: "European euro",
          currencySymbol: "€",
          id: "AD",
          name: "Andorra",
        },
        AE: {
          alpha3: "ARE",
          currencyId: "AED",
          currencyName: "UAE dirham",
          currencySymbol: "فلس",
          id: "AE",
        },
      };

      for (let key in data) {
        this.countries.push(data[key].currencyId);
      }
    },

    async action(url) {
      const result = await axios.get(
        `${api.baseUrl}${url}apiKey=${api.apiKey}`
      );
      if (result.status === 200) {
        return result.data;
      }
      return null;
    },
  },
  computed: {
    dateNow() {
      return new Date().toISOString().slice(0, 10);
    },
    dateTenDays() {
      let d = new Date();
      // к сожалению, апи поддерживает только 8 дней
      d.setDate(d.getDate() - 8);
      return d
        .toISOString()
        .toString()
        .slice(0, 10);
    },
  },
  async mounted() {
    this.getHistoryData();
    this.getPriceTableData();
    this.getCountries();
    this.getHiddenColumns();
  },
  watch: {
    price: function(val) {
      localStorage.setItem("price", val);
    },
    story: function(val) {
      localStorage.setItem("story", val);
    },
  },
};
</script>

<style lang="scss">
body {
  font-size: 14px;
  margin: 0;

  @media (max-width: 768px) {
    font-size: 16px;
  }
}

button {
  background-color: transparent;
  border: none;

  &:hover {
    opacity: 0.6;
  }
}

.main-page {
  display: flex;
  flex-direction: column;
  min-height: calc(100vh - 80px);
  padding: 30px;
  background-color: rgb(245, 245, 245);

  &__container {
    display: flex;

    @media (max-width: 1300px) {
      flex-wrap: wrap;
    }
  }
}

.base-block {
  width: 32%;
  padding: 40px 20px;
  background-color: #fff;
  border-radius: 10px;
  margin-bottom: 20px;

  @media (min-width: 1300px) {
    &:not(:last-child) {
      margin-right: 2%;
    }
  }

  @media (max-width: 1300px) {
    width: 49%;

    &:first-child {
      margin-right: 2%;
    }
  }

  @media (max-width: 950px) {
    width: 100%;

    &:first-child {
      margin-right: 0;
    }
  }

  &__title {
    font-size: 1.3rem;
    margin-top: 0;
  }

  &__field {
    display: flex;
  }

  &--story {
    @media (max-width: 1300px) {
      width: 100%;
    }
  }
}

.country-choose {
  display: flex;
  flex-direction: column;

  @media (max-width: 1300px) {
    width: 45%;
  }

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
