<template>
  <div class="chat">
    <ChatBar /><br><br>
           <v-col class="col-md-6 offset-md-3">
         <v-btn class="blue white--text" @click="$router.push('../users')" title="Vuelta a usuarios">Usuarios</v-btn>
        
        
        <br><br>Chat de {{ userMail }}<br>
    <div class="container-message" v-if="userId && paramId">
      <div class="message" v-for="message in messages" :key="message._id">
        <p v-if="message.userOwner == userId" class="text-right">Yo:</p>
        <p v-else class="indigo--text text--darken-2">{{ message.userTwo }} {{ userMail }}  dice:</p>
         <p v-if="message.userOwner == userId"  class="text-right">{{ message.text }}</p>
        <p v-else>{{ message.text }}</p>

      </div>

      <v-text-field v-model="message"> </v-text-field>
      <v-btn @click="sendMessage"  title="Enviar Mensage" >Enviar</v-btn>
    </div>
           </v-col>
             <ChatFooter />
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
     this.userMail = window.localStorage.getItem('email')

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
