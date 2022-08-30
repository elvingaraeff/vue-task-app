<template>
  <div>
    <p class="label">{{ label }}</p>
    <label :class="{ active: active }" class="toggle__button">
      <span v-if="active" class="toggle__label">{{ labelActive }}</span>
      <span v-else class="toggle__label">{{ labelInactive }}</span>

      <input type="checkbox" v-model="active" />
      <span class="toggle__switch"></span>
    </label>
  </div>
</template>

<script>
export default {
  name: "Toggle",

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
    labelActive: {
      type: String,
      required: true,
    },
    labelInactive: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      active: false,
    };
  },
  methods: {},
  computed: {
    checkedValue: {
      get() {
        return this.active;
      },
      set(newValue) {
        this.active = newValue;
      },
    },
  },
  watch: {
    iteration() {
      this.active = this.data === "active" ? true : false;
    },
    active() {
      let status = this.active ? "active" : "inactive";
      if (status !== this.data) {
        this.$emit("updateValue", status);
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
}
/* Large devices (laptops/desktops, 992px and up) */
@media only screen and (min-width: 992px) {
  div {
    width: 45%;
  }
}
.toggle__button {
  vertical-align: middle;
  user-select: none;
  cursor: pointer;
}
.toggle__button input[type="checkbox"] {
  opacity: 0;
  position: absolute;
  width: 1px;
  height: 1px;
}
.toggle__button .toggle__switch {
  display: inline-block;
  height: 12px;
  border-radius: 6px;
  width: 40px;
  background: #bfcbd9;
  box-shadow: inset 0 0 1px #bfcbd9;
  position: relative;
  margin-left: 10px;
  transition: all 0.25s;
}
.toggle__button .toggle__switch::after,
.toggle__button .toggle__switch::before {
  content: "";
  position: absolute;
  display: block;
  height: 18px;
  width: 18px;
  border-radius: 50%;
  left: 0;
  top: -3px;
  transform: translateX(0);
  transition: all 0.25s cubic-bezier(0.5, -0.6, 0.5, 1.6);
}
.toggle__button .toggle__switch::after {
  background: #4d4d4d;
  box-shadow: 0 0 1px #666;
}
.toggle__button .toggle__switch::before {
  background: #4d4d4d;
  box-shadow: 0 0 0 3px rgba(0, 0, 0, 0.1);
  opacity: 0;
}
.active .toggle__switch {
  background: #adedcb;
  box-shadow: inset 0 0 1px #adedcb;
}
.active .toggle__switch::after,
.active .toggle__switch::before {
  transform: translateX(40px - 18px);
}
.active .toggle__switch::after {
  background: #53b883;
  box-shadow: 0 0 1px #53b883;
}
</style>
