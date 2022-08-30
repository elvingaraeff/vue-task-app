<template>
  <div id="app">
    <h1>Task app</h1>

    <Users
      :users="users"
      :APIUrl="APIUrl"
      :bearerToken="bearerToken"
      @showModal="showModal"
    />
    <transition name="fade" mode="out-in">
      <Modal
        class="modal"
        v-show="displayModal"
        @modalClose="hideModal"
        :iteration="modalIteration"
        :user="chosenUser"
        :type="modalType"
        :APIUrl="APIUrl"
        :bearerToken="bearerToken"
      />
    </transition>
  </div>
</template>

<script>
import Users from "./components/Users.vue";
import Modal from "./components/Modal.vue";

export default {
  name: "App",
  components: {
    Users,
    Modal,
  },
  data() {
    return {
      APIUrl: "https://gorest.co.in/public/v2/users/",
      bearerToken:
        "Bearer 9b08f032c622616a85e2b4ce4a2d378c3b29e8cbe2cdb3485f6cc280c58dac92",
      displayModal: false,
      users: [],
      chosenUser: {},
      modalType: "",
      modalIteration: 0,
    };
  },
  created() {
    this.getAllUsers();
  },
  methods: {
    async getAllUsers() {
      const response = await fetch(this.APIUrl, {
        headers: {
          Authorization: this.bearerToken,
        },
      });
      this.users = await response.json();
    },

    showModal(type, user) {
      this.modalIteration++;

      this.displayModal = true;
      this.modalType = type;
      this.chosenUser = user;
    },

    hideModal() {
      this.getAllUsers();
      this.displayModal = false;
    },
  },
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 20px;
  h1 {
    text-align: center;
  }
}

button {
  margin: 0 30px;
  height: 43px;
  margin-bottom: 0;
  padding: 6px 20px;
  border-radius: 5px;
  border: none;
  color: white;
  font-weight: 500;
  font-size: 1rem;
  line-height: 1.5;
  background-color: #9c9d99;
  &:hover {
    background-color: gray;
    cursor: pointer;
  }
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1050;
  display: none;
  width: 100%;
  height: 100%;
  overflow: hidden;
  outline: 0;
}


.inputs input{
    width: 100% !important;
}
.buttons{
  margin-top: 20px;
  margin-bottom: 20px;
}
.buttons button{
  margin: 0 10px;
  width: 145px;
}
.search span{
  display: none !important;
}
</style>
