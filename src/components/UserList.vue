<template>
  <div class="userinfo-parent-container">
      <section class="user-top-header-section">
          <div class="user-top-info-child-wrapper">
              <div>Users</div>
              <button  @click="modalActivate()" class="blue-action-btn">Create User</button>
          </div>
      </section>
      <section class="user-info-section">
          <table>
  <tr>
    <th v-for="col in tableColumns" :key="col">{{col}}</th>
  </tr>
  <template v-if="userInformation.length>0">
  <tr v-for="user in userInformation" :key="user.id">
  <td>{{user.name}}</td>
  <td>{{user.username}}</td>
  <td>{{user.email}}</td>
  <td>{{user.phone}}</td>
  <td>{{user.status}}</td>
  <td><img  @click="deleteUser(user)" src="@/assets/trashicon.svg" alt="delete" class="small-icon">
  <img src="@/assets/userediticon.svg"  @click="editUser(user)" alt="Edit" class="small-icon"></td>
  </tr>
  </template>
  </table>
  <template v-if="userInformation.length==0">
  <section class="no-results-area">
      <h1>No items</h1>
      <img src="@/assets/notallowed.svg" alt="No" class="small-icon">
  </section>
  </template>
      </section>
      <template v-if="isModalOpen">
      <AdduserModal @close="modalClose" @userAdded="updateUserList"  :role="role" :user="curruser" @userDeleted="removeUser"/>
      </template>
  </div>
</template>

<script>
import axios from "axios";
import  AdduserModal from "./AdduserModal.vue";
export default {
  name: 'UserList',
  components:
  {
      AdduserModal
  },
  data() {
    return {
      userDetails:[],
      tableColumns:["Name","Surname","Email","Phone Number","Activate","Action"],
      isModalOpen:false,
      role:"",
      curruser:{},
    };
  },
  mounted()
  {
      this.getUserList();
  },
  computed:
  {
      userInformation()
      {
         
          return this.userDetails
      }
  },
  methods:
  {
      getUserList()
      {
          axios
      .get('/users.json')
      .then(response => {this.userDetails = response.data
     })
      },
      modalActivate()
      {
          this.isModalOpen=true;
          this.role="add";
      },
      deleteUser(user)
      {
          this.isModalOpen=true;
          this.role="delete";
          this.curruser=user;
      },
      modalClose()
      {
          this.isModalOpen=false;
      },
      updateUserList(user)
      {
          user.id=Math.floor((Math.random() * 1000) + 1);
          this.userDetails.unshift(user);
      },
      removeUser(userId)
      {
          this.userDetails= this.userDetails.filter(info=>{return info.id!=userId});
      },
      editUser(user)
      {
          this.$emit("editAction",user);
      },
      
  }
}
</script>


<style>
.userinfo-parent-container
{
    background-color: #ffffff;
    position: relative;
    top: 40px;
    width: 75vw;
    left: 20px;
    padding-top: 14px;
    padding-bottom: 14px;
    padding-left: 14px;
    padding-right: 10px;
    border: 1px solid lightgrey;
}
.blue-action-btn
{
    background: blue;
    color: white;
    border: none;
    padding: 5px;
    margin-left: auto;
}
.user-top-info-child-wrapper
{
    display: flex;
    align-items: center;
}
.user-top-header-section
{
    border-bottom:1px solid lightgray;
    padding-bottom: 10px;
}
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td, th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
.small-icon
{
    cursor: pointer;
    height: 20px;
    width:20px;
    padding-right:10px;
}
.no-results-area
{
    display: inline-flex;
    gap: 20px;
    align-items: center;
    font-size: 24px;
    width: 100%;
    background-color: #f4f4f4;
    justify-content: center;
}
</style>
