<template>
  <v-container>
    <v-row class="text-center">

      <v-col class="mb-4">
        <h1 class="display-2 font-weight-bold mb-3">
          所得税計算
        </h1>
        <p>給与所得控除、基礎控除、所得税率のみを考慮した概算だよ</p>
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
        <v-form
          ref="form"
          v-model="valid"
        >
          <v-text-field
            v-model="income"
            :rules="incomeRules"
            label="所得"
            required
          ></v-text-field>
          <v-btn
            :disabled="!valid"
            color="success"
            @click="calculate"
          >
            計算
          </v-btn>
        </v-form>
      </v-col>
      <v-col
        cols="12"
      >
        <h2>
          {{result}}
        </h2>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    name: 'Calculation',
    data () {
      return {
        income: null,
        valid: false,
        incomeRules: [
          v => !!v || '所得を入れてね',
          v => (v && !isNaN(v)) || '所得は数値で入れてね',
        ],
        result: null
      }
    },
    methods: {
      calculate() {
        this.result = '¥' + this.tax(this.taxedIncome(this.income)).toLocaleString() + '-'
      },
      taxedIncome(income) {
        let result
        if (income <= 1800000) {
          if (income < 650000) {
            result =  650000 + 380000
          } else {
            result = parseInt(income * 0.4) + 380000
          }
        } else if (income <= 3600000) {
          result = parseInt(income * 0.3) + 560000
        } else if (income <= 6600000) {
          result = parseInt(income * 0.2) + 920000
        } else if (income <= 10000000) {
          result = parseInt(income * 0.1) + 1580000
        } else {
          result = 2580000
        }
        if (result > income) {
          return 0
        } else {
          return income - result
        }
      },
      tax(income) {
        let result
        if (income <= 1950000) {
          result = parseInt(income * 0.05)
        } else if (income <= 3300000) {
          result = parseInt(income * 0.1) - 97500
        } else if (income <= 6950000) {
          result = parseInt(income * 0.2) - 427500
        } else if (income <= 9000000) {
          result = parseInt(income * 0.23) - 636000
        } else if (income <= 18000000) {
          result = parseInt(income * 0.33) - 1536000
        } else if (income <= 40000000) {
          result = parseInt(income * 0.4) - 2796000
        } else {
          result = parseInt(income * 0.45) - 4796000
        }
        return result
      }
    }
  }
</script>
