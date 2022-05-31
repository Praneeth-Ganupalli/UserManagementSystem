<template>
<section class="edit-area">
    <header class="no-visible">User Details Edit Section</header>
    <header>Displaying The Details of the User <strong>{{user.name}}</strong></header>
    <div class="form-container">
   <form @submit="handleSubmit($event)">
  <label for="fname">First name:</label><br>
  <input type="text" id="fname" name="fname" v-model="name" @change="getEditStatus()"><br>
  <label for="lname">Last name:</label><br>
  <input type="text" id="lname" name="lname" v-model="lname" @change="getEditStatus()"><br><br>
  <label for="phone">Phone Number:</label><br>
  <input type="text" id="phone" name="phone" v-model="phoneNumber" @change="getEditStatus()"><br><br>
  <label for="phone">Email</label><br>
  <input type="text" id="email" name="email" v-model="email" @change="getEditStatus()"><br><br>
  <label for="status">Account Status:</label><br>
  <select v-model="status" @change="getEditStatus()">
  <option value="Active">Active</option>
  <option value="Inactive">Inactive</option>
  </select><br><br>
  <input type="submit" class="blue-button" :class="userEdited?'activate':'disable-button'" value="Save">
  <button>Cancel</button>
</form> 
    </div>
    </section>
</template>
<script>
export default {
  name: 'UserDetails',
  props: {
    user: Object
  },
  data() {
    return {
      name:this.user.name,
      email:this.user.email,
      lname:this.user.username,
      phoneNumber:this.user.phone,
      status:this.user.status,
      userEdited:false
    };
  },
  methods:
  {
      getEditStatus()
      {
          let currData={
               name:this.name,
      email:this.email,
      lname:this.lname,
      phoneNumber:this.phoneNumber,
      status:this.status
          }
          let propData={
            name:this.user.name,
            email:this.user.email,
            lname:this.user.username,
            phoneNumber:this.user.phone,
            status:this.user.status
          }
          this.userEdited= JSON.stringify(currData)!==JSON.stringify(propData);
      },
      handleSubmit(e)
      {
          e.preventDefault();
         let currData={
               name:this.name,
      email:this.email,
      username:this.lname,
      phone:this.phoneNumber,
      status:this.status
          }
          this.$emit("updatedUser",currData);
      }
  }
}
</script>
<style>
.form-container label {
    display: inline-block;
    padding-bottom: 10px;
    padding-top: 10px;
    font-weight: bold;
}
.blue-button
{
    padding-right: 20px;
    padding-left: 20px;
    margin-right: 25px;
    color: #fff;
}
.activate
{
     background-color: #001489;
}
.disable-button
{
    background-color: #ccc;;
    cursor: not-allowed;
    color:black;
    border:2px solid #ccc;
}
.no-visible
{
    visibility: hidden;
}
</style>