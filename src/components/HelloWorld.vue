<template>
  <div class="header">
    <div class="content">
      <div class="search-block">
        <input
          type="text"
          placeholder="Поиск вакансии по наименованию"
          class="input-search"
          v-model="vacansy"
        />
        <button class="search-btn">Найти</button>
      </div>
      <div class="block">
        <!-- <select class="selector" @select="sortByCategories" v-model="selected">
          <option disabled value="">Направление</option>
          <option></option>
        </select> -->
        <VSelect :isExpanded="false" :selected="selected" :options="categoryOne" @select="sortByCategories" />

        <select class="selector" v-model="selected">
          <option disabled value="">Филиал</option>
          <option>A</option>
          <option>B</option>
        </select>
        <button class="clear-filter" @click="clearSelected">Сбросить фильтры</button>
      </div>
    </div>
    <div class="main">
      <div class="main-content" v-for="item in filteredProducts" :key="item.id">
        <div class="main-block">
          <button class="btn-d">{{ item.direction }}</button>
          <button class="btn-b">{{ item.branch }}</button>
          <button class="btn-с">{{ item.city }}</button>
        </div>
        <div>
          <h2>{{ item.title }}</h2>
          <p class="data">{{ item.data }}</p>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
import VSelect from "./v-select";
import axios from "axios";
export default {
  components: { VSelect },
  name: "HelloWorld",
  data() {
    return {
      selected: "",
      vacansy: "",
      products: [],
      sortedProducts: [],
      categoryOne: [
        { name: "все", value: "all" },
        { name: "Информационные технологии", value: "it" },
        { name: "Кулинария", value: "cook" },
      ],
    };
  },
  computed: {
    filteredProducts() {
      if (this.sortedProducts.length) {
        return this.sortedProducts;
      } else {
        return this.products;
      }
    },
  },
  methods: {
    clearSelected(){
      this.selected = ''
      this.sortByCategories();
    },
    sortByCategories(direction) {
      let vm= this
      this.sortedProducts = [...this.products];
      if (direction) {
        this.sortedProducts = this.sortedProducts.filter(function (e) {
          vm.selected = direction.name;
          return e.direction === direction.name;
        });
      }
    },
    sortProductsBySearchValue(value) {
      this.sortedProducts = [...this.products];
      if (value) {
        this.sortedProducts = this.sortedProducts.filter(function (item) {
          return item.title.toLowerCase().includes(value.toLowerCase());
        });
      } else {
        this.sortedProducts = this.products;
      }
    },
  },
  mounted() {
    const API_URL = "http://localhost:3000/vacansies";
    axios.get(API_URL).then((response) => {
      console.log(response);
      this.products = response.data;
      if (response.data) {
        this.sortByCategories();
        this.sortProductsBySearchValue(this.vacansy);
      }
    });
  },
  watch: {
    vacansy() {
      this.sortProductsBySearchValue(this.vacansy);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.header {
  display: flex;
  flex-direction: column;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.content {
  display: flex;
  flex-direction: column;
  margin: 0 auto;
  max-width: 1100px;
  width: 100%;
}
.search-block {
  display: flex;
  padding: 0 2%;
}
.input-search {
  background-color: #f6f8f9;
  font-size: 16px;
  border: none;
  height: 56px;
  border-radius: 5px;
  color: #99aab2;
  outline: none;
  width: 75%;
  margin: 2%;
  padding-left: 35px;
}
.search-btn {
  background-color: blue;
  color: white;
  font-size: 16px;
  width: 15%;
  border: none;
  margin: 2%;
  border-radius: 5px;
  outline: none;
}
h2{
  font-weight: 700;
  font-size: 20px;
}
.block {
  display: flex;
  padding: 0 2%;
}
.selector {
  background-color: #f6f8f9;
  font-size: 16px;
  border: none;
  height: 56px;
  border-radius: 5px;
  color: #99aab2;
  outline: none;
  width: 37%;
  margin: 2%;
  padding-left: 5px;
}
.clear-filter {
  background: transparent;
  border: none;
  outline: none;
  font-size: 16px;
  height: 56px;
  margin: auto 0;
  padding: 2%;
  color: #2196f3;
}
/* main */
.main {
  display: flex;
  flex-direction: row;
  max-width: 1100px;
  justify-content: space-between;
  align-items: start;
  width: 100%;
  flex-wrap: wrap;
  margin: 0 auto;
}
.main-content {
  background-color: #f6f8f9;
  padding: 1.5%;
  width: 435px;
  display: flex;
  flex-direction: column;
  align-content: space-between;
  margin: 2% auto;
  border-radius: 10px;
}
.btn-d {
  background-color: #d2f4fe;
  padding: 5px;
  outline: none;
  margin: 1%;
  font-size: 14px;
  font-weight: 400;
  border-radius: 10px;
}
.btn-b {
  background-color: #ffe8d3;
  margin: 1%;  font-size: 14px;
  font-weight: 400;
  padding: 5px;
  outline: none;
  border-radius: 10px;
}
.btn-c {
  background-color: #FEF9D3 !important;
  padding: 5px;  font-size: 14px;
  font-weight: 400;
  margin: 1%;
  outline: none;
  border-radius: 10px;
}
.data {
  margin-top: 50px;
  text-align: justify;
  padding-left: 10px;
}
</style>
