<template>
  <div>
    <span class="label">{{ label }}</span>
    <input v-model="inputData" :placeholder="placeholder" />
  </div>
</template>

<script>
export default {
  name: "Input",

  props: {
    iteration: {
      type: Number,
    },
    data: {
      type: String,
    },
    label: {
      type: String,
    },
    placeholder: {
      type: String,
    },
  },
  data() {
    return {
      inputData: "",
    };
  },
  methods: {},
  watch: {
    iteration() {
      this.inputData = this.data;
    },
    inputData() {
      if (this.inputData !== this.data) {
        this.$emit("updateValue", this.inputData);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
div {
  width: 100%;
  .label {
    font-size: 0.8rem;
    display: block;
    width: 100%;
    color: grey;
    margin-top: 1rem;
    margin-bottom: 10px;
  }
  input {
    width: 150px;
    padding: 0.8rem 1.2rem;
    margin: 0;
    border-radius: 5px;
    border: 1px solid #e3e3e3;
    &:focus-visible {
      box-shadow: 0px 0px 5px rgb(0 0 0 / 20%);
      outline: none;
    }
  }
}
/* Large devices (laptops/desktops, 992px and up) */
@media only screen and (min-width: 992px) {
  div {
    width: 45%;
  }
}
</style>
