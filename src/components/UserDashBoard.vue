<template>
  <main>
    <section class="side-bar">
      <SideNavBar />
    </section>
    <section class="main-content">
    <section class="top-header-info">
      <div class="dashboard-info-class">
        <img src="@/assets/menubar.svg" alt="menu-icon" />
        <div>DashBoard</div>
        <div>Users</div>
      </div>
      <img
        class="profile-logo"
        src="@/assets/userlogo.svg"
        alt="profile-photo"
      />
      <hr class="divider" />
    </section>
    <section class="breadcrumb-content">
      <div class="breadcrumb-path">
        <span>HOME</span>
        <span class="opac-less">Users</span>
      </div>
      <div class="prev-path">
        <img
          class="nav-logo"
          src="@/assets/navicon.svg"
          alt="profile-photo"
        />
        <span class="opac-less">DashBoard</span>
      </div>
      <hr />
    </section>
    <section class="userList">
        <section  v-show="notEditMode">
        <UserList  @editAction="handleEditButton" ref="userListInfo" />
        </section>
        <section  v-if="!notEditMode">
            <UserDetails  :user="selectedUser" @updatedUser="handleUpdateAction"/>
        </section>
    </section>
    </section>
  </main>
</template>

<script>
import SideNavBar from "./SideNavBar.vue";
import  UserList from "./UserList.vue";
import  UserDetails from "./UserDetails.vue";
export default {
  name: "UserDashBoard",
  components: {
    SideNavBar,
    UserList,
    UserDetails
  },
  props: {
    msg: String,
  },
  data() {
    return {
      selectedUser:{},
      notEditMode:true,
      editedUser:{},
    };
  },
  methods:
  {
      handleEditButton(user)
      {
          this.notEditMode=false;
          this.selectedUser=user;
      },
      handleUpdateAction(user)
      {
          
            this.editedUser=user;
            this.$refs.userListInfo.updateUserList(user);
           this.$refs.userListInfo.removeUser(this.selectedUser.id);
           this.notEditMode=true;
      }

  }
};
</script>


<style>
main
{
    display: flex;
}
.side-bar
{
    flex-basis: 20%;
    max-width:20%;
}
.main-content
{
    flex-basis: 80%;
    max-width:80%;
}
.top-header-info img {
  height: 30px;
  width: 30px;
}
.top-header-info .profile-logo {
  position: absolute;
  right: 35px;
}
.top-header-info {
  width: 25%;
  margin-top: 17px;
}
.top-header-info .divider {
  width: 100vw;
}
.dashboard-info-class {
  display: inline-flex;
  gap: 25px;
  align-items: center;
}
.breadcrumb-content {
  width: 16%;
  margin-top: 15px;
}
.breadcrumb-content hr {
  width: 100vw;
}
.breadcrumb-path :first-child {
  color: #000080;
  font-weight: bold;
  padding-right: 0.3rem;
}
.breadcrumb-path {
  padding-bottom: 0.7em;
}
.breadcrumb-path :first-child::after {
  content: "/";
  padding: 0.2em;
}
.prev-path {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  position: absolute;
  right: 17px;
  top: 73px;
}
.prev-path img {
  height: 30px;
  width: 30px;
}
.userList,.userDetail
{
    background-color: #f4f4f4;
    height: auto;
    position: relative;
    bottom: 8px;
}
.opac-less
{
    opacity:0.5;
}
</style>
