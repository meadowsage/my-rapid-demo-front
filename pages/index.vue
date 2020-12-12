<template>
  <v-app id="inspire">
    <v-system-bar app>
      <v-spacer></v-spacer>
      <v-icon>mdi-square</v-icon>
      <v-icon>mdi-circle</v-icon>
      <v-icon>mdi-triangle</v-icon>
    </v-system-bar>

    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>社員リスト</v-toolbar-title>
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" fixed temporary>
      <!--  -->
    </v-navigation-drawer>

    <v-main class="grey lighten-2">
      <v-container>
        <v-row>
          <template v-for="(department, n) in departments">
            <v-col :key="n" class="mt-2" cols="12">
              <strong>{{ department }}</strong>
            </v-col>

            <v-col
              v-for="(employee, j) in getEmployeesOnDepartment(department)"
              :key="`${n}${j}`" cols="6" md="2"
            >
              <v-card elevation="2" min-width="344" outlined>

                <v-list-item three-line>
                  <v-list-item-content>
                    <div class="overline mb-4">
                      {{ department }}
                    </div>
                    <v-list-item-title class="headline mb-1">
                      {{ employee.name }}
                    </v-list-item-title>
                    <v-list-item-subtitle>ここに簡単な説明</v-list-item-subtitle>
                  </v-list-item-content>
                  <v-list-item-avatar tile size="80" color="grey"></v-list-item-avatar>
                </v-list-item>

                <v-card-actions>
                  <v-btn color="primary lighten-2" text>編集</v-btn>
                  <v-btn color="error lighten-2" text>削除</v-btn>
                </v-card-actions>

              </v-card>
            </v-col>
          </template>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      drawer: null,
      employees: []
    }
  },
  computed: {
    departments: function () {
      return this.employees.map(employee => employee.department?.name)
        .filter((value, index, self) => self.indexOf(value) === index)
    }
  },
  async created() {
    const response = await fetch(`${process.env.baseUrl}/api/employees`)
    if (response.ok) {
      this.employees = await response.json()
    } else {
      console.error(response.statusText)
    }
  },
  methods: {
    getEmployeesOnDepartment(department) {
      return this.employees.filter(employee => employee.department.name === department)
    }
  }
}
</script>

<style></style>
