<template>
  <div class="modal">
    <div class="box">
      <h2>
        <span v-if="type == 'create'"> Create user </span>
        <span v-else-if="type == 'find'"> Find user </span>
        <span v-else> User {{ user.id }}</span>
      </h2>

      <div class="inputs">
        <Input
          :iteration="iteration"
          :data="user.name"
          :label="'Name'"
          :placeholder="'Name'"
          @updateValue="name = $event"
        />
        <Input
          :iteration="iteration"
          :data="user.email"
          :label="'Email'"
          :placeholder="'Email'"
          @updateValue="email = $event"
        />
        <Radio
          :iteration="iteration"
          :data="user.gender"
          :label="'Gender'"
          :options="['Female', 'Male']"
          @updateValue="gender = $event"
        />
        <Toggle
          :iteration="iteration"
          :data="user.status"
          :label="'Status'"
          :labelActive="'Active'"
          :labelInactive="'Inactive'"
          @updateValue="status = $event"
        />
      </div>

      <p v-show="createError">{{ createError }}</p>
      <p v-show="showError">Woops! Something went wrong</p>

      <div class="buttons">
        <button v-if="!user.id" type="button" class="save" @click="getUserData">
          Add user
        </button>
        <button
          v-if="user.id && (name || email || gender || status)"
          type="button"
          class="save"
          @click="getUpdateData"
        >
          Save changes
        </button>
        <button
          v-if="user.id"
          type="button"
          class="delete"
          @click="deleteUser(user.id)"
        >
          Delete user
        </button>
        <button type="button" class="close" @click="modalClose">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
import Input from "./Input.vue";
import Radio from "./Radio.vue";
import Toggle from "./Toggle.vue";

export default {
  name: "Modal",
  components: {
    Input,
    Radio,
    Toggle,
  },
  props: {
    iteration: {
      type: Number,
      required: true,
    },
    user: {
      type: Object,
      required: true,
    },
    type: {
      type: String,
      required: true,
    },
    APIUrl: {
      type: String,
      required: true,
    },
    bearerToken: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      userID: "",
      name: "",
      email: "",
      gender: "",
      status: "",
      showError: false,
      createError: "",
    };
  },
  methods: {
    getUserData() {
      if (this.name && this.email && this.gender) {
        let data = {};
        data.name = this.name;
        data.email = this.email;
        data.gender = this.gender;
        data.status = this.status || "inactive";
        this.createError = "false";
        this.createUser(data);
      } else {
        this.createError = "Please fill in all fields";
      }
    },

    async createUser(data) {
      const response = await fetch(this.APIUrl, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Authorization: this.bearerToken,
        },
        body: JSON.stringify(data),
      });
      if (response.status == 201) {
        this.modalClose();
      } else if (response.status == 422) {
        this.createError = "Invalid email";
      }
    },

    getUpdateData() {
      let data = {};
      if (this.name) data.name = this.name;
      if (this.email) data.email = this.email;
      if (this.gender) data.gender = this.gender;
      if (this.status) data.status = this.status;
      this.updateUser(this.user.id, data);
    },

    async updateUser(id, data) {
      const response = await fetch(`${this.APIUrl}${id}`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
          Authorization: this.bearerToken,
        },
        body: JSON.stringify(data),
      });
      if (response.status == 200) {
        this.showError = false;
        this.modalClose();
      } else {
        this.showError = true;
      }
    },

    async deleteUser(id) {
      const response = await fetch(`${this.APIUrl}${id}`, {
        method: "DELETE",
        headers: {
          Authorization: this.bearerToken,
        },
      });
      if (response.status == 204) {
        this.showError = false;
        this.modalClose();
      } else {
        this.showError = true;
      }
    },

    modalClose() {
      this.$emit("modalClose");
      this.name = "";
      this.email = "";
      this.gender = "";
      this.status = "";
      this.showError = false;
      this.createError = "";
    },
  },
  watch: {
    iteration() {
      if (this.type === "find") {
        this.userID = this.user.id;
      }
    },
    userID() {
      this.getUser(this.userID);
    },
  },
};
</script>

<style lang="scss" scoped>
.modal {
  display: block;
  width: 100vw;
  height: 100vh;
  background-color: rgba(94, 94, 94, 0.4);
  backdrop-filter: blur(2px);
  overflow: hidden;

  .box {
    margin: 45vh auto;
    height: auto;
    width: 600px;
    padding: 20px;
    border-radius: 5px;
    background-color: white;
    transform: translateY(-50%);
    box-shadow: 5px 10px 18px #888888;

    h2 {
      text-align: center;
    }
    p {
      font-size: 1.2rem;
      padding-top: 1rem;
    }

    .message {
      padding: 1rem 0;
    }

    .inputs {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-around;
    }

    .buttons {
      display: flex;
      justify-content: center;
      flex-direction: column;

      button {
        &.save {
          background-color: #4dcb96;
        }
        &.delete {
          background-color: #cb4d4d;
        }
        &.close {
          background-color: #9c9d99;
        }
      }
    }
  }
}

/* Small devices (portrait tablets and large phones, 600px and up) */
@media only screen and (min-width: 600px) {
  .modal .box .buttons {
    flex-direction: row;
  }
}

/* Large devices (laptops/desktops, 992px and up) */
@media only screen and (min-width: 992px) {
  .modal .box {
    width: 600px;
    padding: 20px;
    border-radius: 5px;
  }
}
</style>
