<template>
  <div class="signUp">
    <v-text-field v-model="email" placeholder="Email"> </v-text-field>
    <v-text-field v-model="password1" placeholder="Password" type="password"></v-text-field>
    <v-text-field v-model="password2" placeholder="Repita el Password" type="password">
    </v-text-field>
    <v-btn block elevation="2" x-large @click="onClick">Enviar</v-btn>
  </div>
</template>


<script>
export default {
  data () {
    return {
      email: '',
      password1: '',
      password2: '',
    }
  },
  methods: {
    async onClick () {
      if (!this.email || !this.password1 || !this.password2 || this.password1 !== this.password2) {
        alert('Error en email o password')
        return
      }
      try {
        const url = 'http://localhost:4500/user/signUp'
        const body = {
          email: this.email,
          password1: this.password1,
          password2: this.password2,
        }
        const res = await fetch(url, {
          method: 'post',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(body)
        })
        const data = await res.json()
        if (data.error) {
          alert(data.error)
        } else {
          console.log({ data }) // data
          this.$router.push('/login')
        }
      } catch (err) {
        alert('Hubo un error al crear tu usuario')
      }
    }
  }
}
</script>