<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-list three-line>
        <v-subheader key="header" v-text="`All Transactions`"></v-subheader>
        <template v-for="(item, index) in allTransactions">
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
      </v-list></v-col
    ></v-row
  >
</template>

<script>
export default {
  data() {
    return {
      allTransactions: [],
    }
  },
  mounted() {
    this.getAllTransactions()
  },
  methods: {
    getRandomColor() {
      const colors = ['indigo', 'green', 'red', 'blue']
      const color = colors[Math.floor(Math.random() * colors.length)]
      return color
    },
    async getAllTransactions() {
      try {
        const response = await this.$axios.get('/transactions/all')
        this.allTransactions = response.data
      } catch (error) {
        console.log(error)
      }
    },
  },
}
</script>

<style lang="scss" scoped></style>
