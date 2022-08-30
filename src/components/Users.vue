<template>
  <div class="container">
    <div class="actions">
      <div class="search">
        <p>Find user</p>
        <Input :placeholder="'User ID'" @updateValue="userID = $event" />
      </div>
      <button type="button" @click="getUser(userID)">Search</button>
      <p v-show="findError">{{ findError }}</p>
    </div>

    <div class="users">
      <div
        class="user"
        :class="{ inactive: user.status === 'inactive' }"
        v-for="(user, i) in users"
        :key="i + 'user'"
        @click="showModal('update', user)"
      >
        <span>{{ user.id }}</span>
        <h3>{{ user.name }}</h3>
        <p>{{ user.email }}</p>
        <p>{{ user.gender }}</p>
      </div>
    </div>
    <div class="actions">
      <button type="button" @click="showModal('create', {})">Add user</button>
    </div>
  </div>
</template>

<script>
import Input from "./Input.vue";

export default {
  name: "Users",
  components: {
    Input,
  },
  props: {
    users: {
      type: Array,
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
      userID: 0,
      findError: "",
    };
  },

  methods: {
    async getUser(id) {
      const response = await fetch(`${this.APIUrl}${id}`, {
        headers: {
          Authorization: this.bearerToken,
        },
      });
      if (response.status == 200) {
        this.showModal("update", await response.json());
        this.findError = "";
      } else if (response.status == 404) {
        this.findError = "Invalid user ID";
      }
    },
    showModal(type, user) {
      this.$emit("showModal", type, user);
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  align-items: center;
  flex-direction: column;
  .actions {
    width: 600px;
    padding: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    .search {
      display: flex;
      flex-direction: row;
      align-items: center;
      div input {
        margin-bottom: 0;
      }
    }
    button {
      width: auto;
    }
    p {
      width: auto;
      margin-left: 0;
      padding:0;
      margin: 0;
    padding-right: 30px;
    }
  }
}

.users {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  width: 600px;
}
.user {
  display: flex;
  justify-content: space-between;
  flex-direction: row;
  align-items: center;
  width: 100%;
  margin: 5vw;
  padding: 10px 20px;
  font-size: 0.9rem;
  word-break: break-word;
  background: #0491ca;
  color: white;
  border-radius: 5px;
  transition: transform 0.2s;

  &:hover {
    cursor: pointer;
    transform: scale(1.05);
    filter: brightness(1.2);
  }

  &.inactive {
    background: #9c9d99;
  }
  span{
    display: inline-block;
    width: 50px;
  }
  h3 {
    font-size: 1.1rem;
    margin: 0;
    width: 45%;
  }

  p{
    text-align: right;
    width: 25%;
  }

  h3 + p{
    text-align: left;
    width: 30%;
  }

  p {
    margin: 0;
  }
}

/* Small devices (portrait tablets and large phones, 600px and up) */
@media only screen and (min-width: 600px) {
  .container {
    .actions {
      .search {
        display: flex;
        flex-direction: row;
        div input {
          margin-bottom: 0;
        }
      }
      button {
        margin-top: 0;
      }
    }
  }
  .user {
    width: 100%;
    margin: 5px;
  }
}

/* Large devices (laptops/desktops, 992px and up) */
@media only screen and (min-width: 992px) {
  .users {
    width: 1240px;
    padding: 0 20px;
  }
  .user {
    width: 100%;
  }
}
</style>
