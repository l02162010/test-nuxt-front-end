<template>
  <b-container>
    <b-row>
      <b-col cols="3">
        <b-button
          v-if="!isEdit"
          variant="warning"
          @click="isEdit = true">
          編輯
        </b-button>
        <b-button
          v-if="isEdit"
          variant="success"
          @click="updateUser()">
          完成
        </b-button>
      </b-col>
    </b-row>
    <b-row>
      <b-col>
        name: {{ userInfo.name }}
        <b-form-input
          v-if="isEdit"
          v-model="userInfo.name"
          type="text"
          required
          placeholder="輸入名稱"/>
      </b-col>
      <b-col>
        gender:
        <b-form-radio-group
          id="radios2"
          v-model="userInfo.gender">
          <b-form-radio
            v-for="gender in genderOption"
            :key="gender.label"
            :value="gender.value"
            disabled>{{ gender.label }}</b-form-radio>
        </b-form-radio-group>
      </b-col>
      <b-col>
        birthday:
        {{ userInfo.birthday.year }}/{{ userInfo.birthday.month }}/{{ userInfo.birthday.date }}
        <b-row v-if="isEdit">
          <b-col>
            <b-form-select
              v-model="userInfo.birthday.year"
              :options="yearOption"
              class="mb-3" />
          </b-col>
          <b-col>
            <b-form-select
              v-model="userInfo.birthday.month"
              :options="monthOption"
              class="mb-3" />
          </b-col>
          <b-col>
            <b-form-select
              v-model="userInfo.birthday.date"
              :options="dateOption"
              class="mb-3" />
          </b-col>
        </b-row>
      </b-col>
    </b-row>
    <b-row>
      <b-col>
        email: {{ userInfo.email }}
      </b-col>
      <b-col>
        phone: {{ userInfo.phone }}
        <b-form-input
          v-if="isEdit"
          v-model="userInfo.phone"
          type="text"
          required
          placeholder="輸入電話"/>
      </b-col>
      <b-col>
        score: {{ userInfo.score }}
      </b-col>
    </b-row>
    <b-row>
      <b-col>
        描述: <p>{{ userInfo.description }}</p>
        <b-form-textarea
          v-if="isEdit"
          v-model="userInfo.description"
          :rows="3"
          :max-rows="6"
          placeholder="描述你的理想對象..."/>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      url: 'http://localhost:4000',
      userInfo: {
        name: '',
        gender: '',
        birthday: {
          year: 1960,
          month: 1,
          date: 1
        },
        email: 'old@old.com',
        phone: '',
        score: 0,
        love: 0,
        description: ''
      },
      isEdit: false,
      token: '',
      yearOption: [],
      monthOption: [],
      dateOption: [],
      genderOption: [
        {
          label: '紳士',
          value: '1'
        },
        {
          label: '女士',
          value: '0'
        }
      ]
    }
  },
  watch: {
    'userInfo.birthday.month': function(val) {
      this.dateOption = []
      if (val === 2) {
        for (let index = 1; index <= 29; index++) {
          this.dateOption.push({
            text: index,
            value: index
          })
        }
      } else {
        for (let index = 1; index <= 31; index++) {
          this.dateOption.push({
            text: index,
            value: index
          })
        }
      }
    }
  },
  mounted() {
    this.getUsers()
    this.setBirthdayOption()
    this.token = localStorage.getItem('token')
  },
  methods: {
    async getUsers() {
      this.$axios.setHeader('Authorization', localStorage.getItem('token'))
      let res = await this.$axios.$get(this.url + '/api/user')
      console.log(res)
      if (res) {
        console.log(res.data)
        this.userInfo = {
          ...this.userInfo,
          ...res.data
        }
      }
    },
    async updateUser() {
      this.$axios.setHeader('Authorization', this.token)
      console.log(this.userInfo)
      let data = this.userInfo
      console.log(data)
      let res = await this.$axios.$put(this.url + '/api/user', this.userInfo)
      if (res) {
        this.isEdit = false
      }
      console.log(res)
    },
    setBirthdayOption() {
      let now = new Date()
      let nowYear = now.getFullYear()
      let options = []
      for (let index = 1940; index <= nowYear; index++) {
        this.yearOption.push({
          text: index,
          value: index
        })
      }
      for (let index = 1; index <= 12; index++) {
        this.monthOption.push({
          text: index,
          value: index
        })
      }
      for (let index = 1; index <= 31; index++) {
        this.dateOption.push({
          text: index,
          value: index
        })
      }
    }
  }
}
</script>

<style>
</style>
