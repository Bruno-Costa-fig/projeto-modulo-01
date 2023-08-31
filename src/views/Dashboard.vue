<template>
  <div>
    <h1>Dashboard</h1>
    {{ dashboardInfo }}
    {{ userInfo }}
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      dashboardInfo: {},
      userInfo: {}
    }
  },
  mounted(){
    axios.get('http://localhost:3000/dashboard')
    .then(res => this.dashboardInfo = res.data)
    .catch(error => console.log(error))

    this.userInfo = JSON.parse(localStorage.getItem('user-info')) || null
  }
}
</script>