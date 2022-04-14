<template>
  <div class="v-select">
    <p class="title" @click="areOptionsVisible = !areOptionsVisible" v-if="selected">{{ selected}}</p>
    <p class="title" @click="areOptionsVisible = !areOptionsVisible" v-else>Направление</p>
    <div class="options" v-if="areOptionsVisible || isExpanded">
      <p v-for="option in options" :key="option.value" @click="selectOption(option)">
        {{ option.name }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "v-select",
  props: {
    options: {
      type: Array,
      default() {
        return [];
      },
    },
    selected: {
      type: String,
      default: "",
    },
    isExpanded: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      areOptionsVisible: false,
    };
  },
  methods: {
    selectOption(option) {
      this.$emit("select", option);
      this.areOptionsVisible = false;
    },
    hideSelect() {
      this.areOptionsVisible = false;
    },
  },
  mounted() {
    document.addEventListener("click", this.hideSelect.bind(this), true);
  },
  beforeDestroy() {
    document.removeEventListener("click", this.hideSelect);
  },
};
</script>

<style>
/* .selector {
  background-color: #f6f8f9;
  height: 56px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  color: #99aab2;
  outline: none;
  width: 37%;
  margin: 2%;
  padding-left: 5px;
} */

.v-select {
  position: relative;
  width: 350px;
  cursor: pointer;
  background-color: #f6f8f9;
  height: 56px;
  font-size: 16px;
  outline:none;
  border: none;
  margin: 2%;
  border-radius: 5px;
  text-align: left;
}
.title {
  outline:none;
  border: none;  padding: 8px;
  color: #99aab2;
margin:  auto 2%;
padding-top: 20px;
}
.v-select p {
  margin: 0;
}
.options {
  border: solid 1px #aeaeae;
  background: #ffffff;
  position: absolute;
  top: 30px;
  left: 0;
  width: 100%;
  margin-top: 4%;
  padding: 6px 8px 8px;
}
.options p:hover {
  background: #e7e7e7;
}
</style>
