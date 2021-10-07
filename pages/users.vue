<template>
  <div class="users">
    <ChatBar />
      <v-col class="col-md-6 offset-md-3">
    <div>
      <div>
        <p class="px-4 pt-4 pb-3 font-weight-medium"><br><br>
          Usuario {{ email }} conectado.  </p><p v-if="isAdmin"> Eres Admin</p>
    
      </div>
    </div>
    <br /><br />¿Con quién quieres chatear?
    <hr />
    <br />
    <div v-for="user in users" :key="user._id">
      <p class="text-center">
        {{ user.email }}&nbsp;&nbsp;&nbsp;
        <v-btn
          class="green white--text"
          title="Iniciar Chat"
          @click="redirect(user._id)"
        >
          Iniciar chat</v-btn
        >&nbsp;&nbsp;
        <v-btn v-if="isAdmin"
          class="red white--text"
          title="Eliminar Usuario"
          @click="suprime(user._id)"
        >
          X
        </v-btn>
        
      </p>
    </div>

    <div>
      <v-btn @click="logout()" class="red white--text" title="Logout">
        Logout</v-btn
      >
    </div>
      </v-col>
          <ChatFooter />
  </div>
  
</template>

<script>
export default {
  data() {
    return {
      users: [],
      user: '',
      isAdmin: '',
      email: undefined,
    }
  },

  // si no tiene token le echamos a login
  async beforeMount() {
    const token = window.localStorage.getItem('token')

    if (!token) {
      this.$router.push('/login')
    }
    await this.getAllUsers(token)
    //reviso si es admin
    this.isAdmin = window.localStorage.getItem('admin') === 'true'
    this.email = window.localStorage.getItem('email')
  },
  methods: {
    redirect(id) {
      this.$router.push(`/chat/${id}`)
    },

    async suprime(id) {
      await fetch('http://localhost:4500/user/suprime/' + id, {
        method: 'delete',
      })
     location.reload(); 
    },


    logout() {
      const token = window.localStorage.getItem('token')
      window.localStorage.removeItem('token')
      this.$router.push('/login')
    },

    async getAllUsers(token) {
      try {
        const res = await fetch('http://localhost:4500/user/all', {
          headers: {
            token,
          },
        })
        const data = await res.json() //creo la data que debo llenar
        console.log({ data }) //imprimo la data

        if (data.error) {
          alert(data.error)
          return this.$router.push('/login')
        }

        const users = data.users //creo la constante users que esta dentro de la data.users. Ahora solo lo llamaré como users
        for (let i = 0; i < users.length; i++) {
          this.users.push(users[i])
        }
      } catch (err) {
        alert(err)
      }
    },
  },
}
</script>
