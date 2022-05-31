<template>
  <div class="pop-up-modal">
    <div class="mask active" role="dialog"></div>
    <template v-if="role == 'add'">
      <div class="modal add-modal" role="alert">
        <form @submit="validateForm($event)">
          <label class="align-ele"> Firstname </label>
          <input
            required
            type="text"
            name="firstname"
            size="15"
            v-model="name"
          />
          <br />
          <br />
          <label class="align-ele"> Lastname: </label>
          <input
            required
            type="text"
            name="lastname"
            size="15"
            v-model="username"
          />
          <br />
          <br />
          <label class="align-phone"> Phone : </label>
          <input
            required
            type="tel"
            name="phone"
            size="10"
            v-model="phoneNumber"
          />
          <br />
          <br />
          <label class="align-ele"> Email: </label>
          <input
            required
            type="email"
            id="email"
            name="email"
            v-model="email"
          />
          <br />
          <br />
          <br />
          <input type="submit" value="Submit" />
        </form>
        <button class="close"  @click="closePopup()" role="button">X</button>
      </div>
</template>
<template v-if="role=='delete'">
  <div class="modal delete-modal" role="alert">
    <p class="delete-info-content">
      You are about to Delete Record of the User Name <b>{{ user.name }}</b>
    </p>
    <div class="button-group">
      <button  @click="closePopup()" >Cancel</button>
      <button @click="deleteUser()">Delete</button>
    </div>
    <button class="close"  @click="closePopup()" role="button">X</button>
  </div>
</template>
</div>
</template>
<script>
export default {
  name: "AdduserModal",
  props: {
    modalFlag: Boolean,
    role: String,
    user: Object,
  },
  data() {
    return {
      name: "",
      username: "",
      phoneNumber: "",
      email: "",
    };
  },
  methods: {
    closePopup() {
      this.$emit("close");
    },
    validateForm(e) {
      e.preventDefault();
      let newUser = {
        username: this.username,
        name: this.name,
        phone: this.phoneNumber,
        email: this.email,
        status: "Inactive",
      };
      this.$emit("userAdded", newUser);
      this.$emit("close");
    },
    deleteUser()
    {
        this.$emit("userDeleted",this.user.id);
        this.$emit("close");
    }
  },
};
</script>
<style>
form {
  margin-top: 45px;
}
.align-ele {
  position: relative;
  left: -5px;
}
.align-phone {
  position: relative;
  left: -32px;
}
.close {
  position: absolute;
  top: 0;
  right: 0;
  width: 35px;
  height: 30px;
  background: #000;
  color: #fff;
  cursor: pointer;
  border: 0;
}
.mask {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(52, 73, 94, 0.8);
  z-index: 50;
  visibility: hidden;

  opacity: 0;

  transition: 0.7s;
}
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  width: 400px;
  height: 300px;
  margin-left: -200px;
  margin-top: -150px;
  background: #bdc3c7;
  z-index: 100;
  visibility: hidden;

  opacity: 0;

  transition: 0.5s ease-out;

  transform: translateY(45px);
}

.active {
  visibility: visible;
  opacity: 1;
}
.active + .modal {
  visibility: visible;
  opacity: 1;
  transform: translateY(0);
}
.delete-modal {
  background-color: #f4f4f4;
}
.add-modal {
  background-color: aqua;
}
.delete-info-content
{
    margin-top:110px;
}
</style>
