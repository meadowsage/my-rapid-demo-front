<template>
  <div class="container">
    <ul>
      <li v-for="employee in employees">
        <span>{{ employee.name }} [{{ employee.department.name }}]</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      employees: []
    }
  },
  async created() {
    const response = await fetch(`${process.env.baseUrl}/api/employees`)
    if (response.ok) {
      this.employees = await response.json()
    } else {
      console.error(response.statusText)
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
  'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
