<template>
  <header class="main-header">
    <h1 class="main-header__title">{{title}}</h1>

    <!-- <button class="table-advance__btn" @click="openColumnPopup = !openColumnPopup">
        <div v-if="openColumnPopup"><i class="far fa-angle-up"></i></div>
        <div v-else-if="!openColumnPopup"><i class="far fa-chevron-down"></i></div>
    </button>
    <div class="table-advance__popup-overlay" v-if="openColumnPopup"  @click.self="outside"></div>
    <div class="table-advance__popup" v-if="openColumnPopup">
        <div class="table-advance__btn-group">
            <label class="table-advance__item" v-for="(item, index) in columnsOriginal">
                <v-checkbox
                    v-model="item.hidden"
                    :value="item"
                    :true-value="0"
                    :false-value="1"
                    :label="item.label"
                    @change="setColumnsToStorage"
                ></v-checkbox>
            </label>
        </div>
    </div> -->
  </header>

</template>

<script>
  export default {
    data() {
      return {
        //title: false
      };
    },
    name: "TopHeader",
    props: {
      title:{
        type: String,
        default: 'Заголовок!'
      },
    },
    // methods: {
    //   setColumnsToStorage() {
    //     const columnString = JSON.stringify(
    //       this.columnsOriginal.map(col => {
    //         return { value: col.value, hidden: col.hidden }
    //       }),
    //     )
    //     localStorage.setItem(`client-base-cols`, columnString)
    //     this.updateHiddenColumnsArray()
    //   },
    // },
  };
</script>

<style lang="scss">
  .main-header {
    display: flex;
    width: 100%;
    background-color: #fff;
    min-height: 40px;
    padding: 0 30px;
    box-shadow: 0px 13px 22px -7px rgba(0, 0, 0, 5%);

    &__title {
      font-size: 1.2rem;

    }
  }
</style>