<template>
  <div class="login">
    <v-text-field v-model="email" placeholder="Email"> </v-text-field>
    <v-text-field v-model="password" placeholder="Password" type="password"></v-text-field>
 
    <v-btn block elevation="2" x-large @click="onClick">Enviar</v-btn>
    <br>  <p>¿No tienes usuario? <nuxt-link to="/signup">Regístrate</nuxt-link></p>
  </div>
</template>

<script>
export default {
  data () {
    return {
      email: '',
      password: '',
    }
  },
  methods: {
    async onClick () {
      const url = 'http://localhost:4500/user/login'
      const body = {
        email: this.email,
        password: this.password,
                    }
      try {
        const res = await fetch(url, {
          method: 'post',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(body),
        })
        const data = await res.json()
        if (data.error) {
          alert(data.error)
          return
        }
      
        window.localStorage.setItem('token', data.token)
        window.localStorage.setItem('userId', data.userId)
        window.localStorage.setItem('admin', data.admin)
        window.localStorage.setItem('email', data.email)
        this.$router.push('/users')
      } catch (err) {
      }
    }
  }
}
</script>