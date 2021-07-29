<template>
  <main class="main-page">
    <div class="main-page__container">
      <section class="base-block">
        <h3 class="base-block__title">Конвертация валют</h3>
        <div>
          <div>
            <select class="select-country" @change="updateInputs" v-model="countrySelect">
              <option v-for="country in countries" :key="country">
                <p>{{country}}</p>
              </option>
            </select>
            <input v-model="firstVal" />
          </div>
        </div>
      </section>
      <section class="base-block">
        <h3 class="base-block__title">Конвертация валют</h3>

      </section>
      <section class="base-block">
        <h3 class="base-block__title">Конвертация валют</h3>

      </section>
    </div>
  </main>

</template>

<script>
import axios from 'axios';

  export default {
    data() {
      return {
        firstVal: 0,
        secondVal: 0,
        countrySelect: [],
        countries: [
          'USA',
          'RUS',
        ]
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

.main-page {
  min-height: calc(100vh - 128px);
  padding: 30px;

  &__container {
    display: flex;
  }
}

.base-block {
  width: 30%;
  padding: 20px;

  &__title {
    font-size: 1.3rem;
  }
}
</style>