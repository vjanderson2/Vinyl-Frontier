<template>
  <div class="dashboard">
    <h1>Welcome back, {{this.$store.state.user.username}}!</h1>
    <h2>You are now viewing your dashboard.</h2>
    <div class = "dashboard-nav">
      <router-link :to="{ name: 'addCollection' }">Create Collection</router-link>
      <router-link :to="{ name: 'addRecord' }">Add Record To Library</router-link>
    </div>
  </div>
</template>

<script>
import recordService from "@/services/RecordService.js";
import collectionService from "@/services/CollectionService.js"

export default {
  name: "home",
  data() {
    return {
      records: [],
      collections: [],
    };
  },
  created() {
    recordService.getAllRecords(this.$store.state.user.id)
    .then((response) => 
      {
        //if code 200
        this.records = response.data;
        this.$store.commit('SET_USER_RECORDS', this.records);
      }
    )
    collectionService.getAllCollections().then((response) => {
      this.allCollections = response.data;
      this.$store.commit('UPDATE_ALL_COLLECTIONS', this.allCollections);
    })
    collectionService.getCollectionsByUserId(this.$store.state.user.id)
    .then((response) =>
      {
        //if ^
        this.collections = response.data;
        this.$store.commit('SET_USER_COLLECTIONS', this.collections);
      }
    )
  }
};
</script>

<style>
.dashboard{
  font-size: 40;
  background-color: rgba(255,255,255,0.6);
  margin-top: 20vh;
  margin-left: 29vw;
  margin-right: 29vw;
  border-radius: 20px;
  display:flex;
  flex-direction: column;
  padding-bottom: 20px;
}

.dashboard > h1, h2 {
  font-family: monospace, sans-serif;
  display: flex;
  justify-content: center;
}

.dashboard > h2 {
  text-align: center;
}

.dashboard-nav {
  font-family: monospace, sans-serif;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 20px;
}

.dashboard-nav > a {
  border: 2px solid black;
  border-radius: 10px;
  background-color: white;
  text-decoration: none;
  padding: 20px;
}

.dashboard-nav > a:hover {
  color: white;
  border-color: white;
  background-color: black;
  padding: 20px;
}
</style>
