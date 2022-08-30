<template>
  <div>
    <p class="label">{{ label }}</p>
    <div class="inputs">
      <label
        class="container"
        v-for="(option, i) in options"
        :key="i + 'option'"
        >{{ option }}
        <input
          type="radio"
          name="radio"
          v-model="choise"
          :value="option.toLowerCase()"
        />
        <span class="checkmark"></span>
      </label>
    </div>
  </div>
</template>

<script>
export default {
  name: "Radio",

  props: {
    iteration: {
      type: Number,
      required: true,
    },
    data: {
      type: String,
    },
    label: {
      type: String,
      required: true,
    },
    options: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      choise: "",
    };
  },
  methods: {},
  watch: {
    iteration() {
      this.choise = this.data;
    },
    choise() {
      if (this.choise !== this.data) {
        this.$emit("updateValue", this.choise);
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
    color: grey;
    margin-top: 1rem;
  }
  .inputs span {
    margin-right: 1rem;
  }
}

/* Customize the label (the container) */
.container {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 12px;
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

/* Hide the browser's default radio button */
.container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

/* Create a custom radio button */
.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 1.2rem;
  width: 1.2rem;
  background-color: #eee;
  border-radius: 50%;
}

/* On mouse-over, add a grey background color */
.container:hover input ~ .checkmark {
  background-color: #ccc;
}

/* When the radio button is checked, add a blue background */
.container input:checked ~ .checkmark {
  background-color: #53b883;
}

/* Show the indicator (dot/circle) when checked */
.container input:checked ~ .checkmark:after {
  display: block;
}

/* Style the indicator (dot/circle) */
.container .checkmark:after {
  top: 9px;
  left: 9px;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: white;
}
/* Large devices (laptops/desktops, 992px and up) */
@media only screen and (min-width: 992px) {
  div {
    width: 45%;
  }
}
</style>
