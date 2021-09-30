<template>

  <div class="users">
      <div><p class='Heading 2'> Usuario {{ users.email }} conectado.  </p> <br><br>¿Con quién quieres chatear?<hr><br></div>
          <div v-for="user in users" :key="user._id">
      <p class="text-center">{{ user.email }} 
          <v-btn class="green white--text" @click="redirect(user._id)"> Iniciar chat</v-btn> 
          <v-btn v-if="user.admin == 'true'" class="red white--text" @click="suprime(user._id)"> X </v-btn>
      </p>
  </div>
    
    <div><v-btn @click="logout()" class="red white--text"> Logout</v-btn></div>
    

  </div>
</template>

<script>
export default {
  data() {
    return {
      users: [],
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
  },
  methods: {
    redirect(id) {
      this.$router.push(`/chat/${id}`)
    },

     async suprime(id) {
      const url = `http://localhost:4500/user/suprime/${id}`;
      await fetch(url, {
          method: 'delete',
          headers: {
            token: this.token,
          },
        });
    }, 


    logout(){
        const token = window.localStorage.getItem('token')
        window.localStorage.removeItem('token');
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
        console.log( {data} ) //imprimo la data
       
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
