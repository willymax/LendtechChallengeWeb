<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card class="mx-auto">
        <v-card-text>
          <h1 class="text-h4 text--primary">
            Hello
            <span
              ><b>{{ $auth.user.firstName }}</b></span
            >
          </h1>
          <h1 class="text-h5 text--primary">Your Balance</h1>
          <p></p>
          <div class="text--primary">
            <h2>Ksh. {{ userBalance }}</h2>
          </div>
        </v-card-text>
        <v-card-actions>
          <v-btn text color="deep-purple accent-4" to="/all-transactions">
            View All Transactions
          </v-btn>
        </v-card-actions>
      </v-card>
      <v-list three-line>
        <v-subheader key="header" v-text="`Recent Transactions`"></v-subheader>
        <template v-for="(item, index) in recentTransactions">
          <v-divider
            v-if="item.divider"
            :key="index"
            :inset="item.inset"
          ></v-divider>

          <v-list-item v-else :key="item.title">
            <v-list-item-avatar>
              <v-avatar :color="getRandomColor()" size="62">
                <span class="white--text text-h5">{{
                  item.debitUser
                    ? item.debitUser.firstName[0]
                    : item.creditUser.firstName[0]
                }}</span>
              </v-avatar>
            </v-list-item-avatar>

            <v-list-item-content>
              <span v-if="item.transactionType == `Sent`">
                <v-list-item-title>
                  To: {{ item.debitUser.firstName }}
                  {{ item.debitUser.lastName }}
                </v-list-item-title>
                <v-list-item-subtitle
                  >Date: {{ item.transactionDateTime }}</v-list-item-subtitle
                >
                <v-list-item-subtitle
                  >Amount: - {{ item.amount }} KES</v-list-item-subtitle
                >
              </span>
              <span v-else>
                <v-list-item-title>
                  From: {{ item.creditUser.firstName }}
                  {{ item.creditUser.lastName }}
                </v-list-item-title>
                <v-list-item-subtitle
                  >Date: {{ item.transactionDateTime }}</v-list-item-subtitle
                >
                <v-list-item-subtitle
                  >Amount: + {{ item.amount }} KES</v-list-item-subtitle
                >
              </span>
            </v-list-item-content>
          </v-list-item>
        </template>
      </v-list>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      userBalance: 0,
      recentTransactions: [],
    }
  },
  mounted() {
    this.getBalance()
    this.getRecentTransactions()
  },
  methods: {
    getRandomColor() {
      const colors = ['indigo', 'green', 'red', 'blue']
      const color = colors[Math.floor(Math.random() * colors.length)]
      return color
    },
    async getBalance() {
      try {
        const response = await this.$axios.get('/user/balance')
        this.userBalance = response.data
        console.log('userBalance', this.userBalance)
      } catch (error) {
        console.log(error)
      }
    },
    async getRecentTransactions() {
      try {
        const response = await this.$axios.get('/transactions/recent')
        this.recentTransactions = response.data
        console.log('recentTransactions', this.recentTransactions)
      } catch (error) {
        console.log(error)
      }
    },
  },
}
</script>

<style lang="scss" scoped></style>
