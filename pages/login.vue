<template>
  <v-app id="inspire">
    <v-content>
      <v-container fluid fill-height>
        <v-layout align-center justify-center>
          <v-flex xs12 sm8 md4>
            <form @submit.prevent="userLogin">
              <v-card class="elevation-12">
                <v-toolbar dark color="primary">
                  <v-toolbar-title>Login form</v-toolbar-title>
                </v-toolbar>
                <v-card-text>
                  <v-form>
                    <v-text-field
                      v-model="login.username"
                      name="login"
                      label="Username"
                      type="text"
                    ></v-text-field>
                    <v-text-field
                      id="password"
                      v-model="login.password"
                      name="password"
                      label="Password"
                      type="password"
                    ></v-text-field>
                  </v-form>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="primary" type="submit">Login</v-btn>
                </v-card-actions>
              </v-card>
            </form>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
export default {
  auth: false,
  data() {
    return {
      login: {
        username: 'willymax',
        password: 'password',
      },
    }
  },
  methods: {
    async userLogin() {
      console.log('userLogin')
      try {
        const response = await this.$auth.loginWith(`local`, {
          data: this.login,
        })
        this.$router.push('/')
        console.log(response)
      } catch (err) {
        console.log(err)
      }
    },
  },
}
</script>

<style lang="scss" scoped></style>
