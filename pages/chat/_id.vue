<template>
  <div class="chat">
        <nuxt-link to="../users">Back to Users</nuxt-link>
    <div class="container-message" v-if="userId && paramId">
      <div class="message" v-for="message in messages" :key="message._id">
        <p v-if="message.userOwner == userId" class="text-right">Yo:</p>
        <p v-else class="indigo--text text--darken-2">{{ message.userTwo }} dice:</p>
         <p v-if="message.userOwner == userId"  class="text-right">{{ message.text }}</p>
        <p v-else>{{ message.text }}</p>

      </div>

      <v-text-field v-model="message"> </v-text-field>
      <v-btn @click="sendMessage">Submit</v-btn>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      messages: [],
      paramId: undefined,
      userId: undefined,
      message: '',
    }
  },

  async beforeMount() {
    this.paramId = this.$route.params.id
    this.userId = window.localStorage.getItem('userId')

    const token = window.localStorage.getItem('token')
    if (!token) {
      this.$router.push('/login')
    }

    await this.getMessages(token)
  },
  methods: {
    async sendMessage() {
      try {
        const token = window.localStorage.getItem('token')
        const userOneId = window.localStorage.getItem('userId')
        const userTwoId = this.$route.params.id
        const body = JSON.stringify({
          userOneId,
          userTwoId,
          userOwnerId: userOneId,
          text: this.message,
        })

        const res = await fetch('http://localhost:4500/message/create', {
          method: 'post',
          headers: {
            token,
            'Content-Type': 'application/json',
          },
          body,
        })

        const data = await res.json()
        if (data.error) {
          alert(data.error)
        } else {
          this.messages = []
          await this.getMessages(token)
        }
      } catch (err) {
        alert(err)
      }
    },

    async getMessages(token) {
      try {
        const userOneId = window.localStorage.getItem('userId')
        const userTwoId = this.$route.params.id

        const body = JSON.stringify({
          userOneId,
          userTwoId,
        })

        const res = await fetch('http://localhost:4500/message/chat', {
          method: 'post',
          headers: {
            'Content-Type': 'application/json',
            token,
          },
          body,
        })

        const data = await res.json() //llenamos la data con a info que nos devuelve el server
        if (data.error) {
          alert(data.error)
          return this.$router.push('/login')
        }

        const messages = data.messages
        for (let i = 0; i < messages.length; i++) {
          this.messages.push(messages[i])
        }

        console.log({ data })
      } catch (err) {
        alert(err)
      }
    },
  },
}
</script>
