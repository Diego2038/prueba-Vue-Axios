import axios from 'axios';
<template>
  <input type="text" placeholder="Email" v-model="email">
  <input type="text" placeholder="Password" v-model="password">
  <button @click="auth">Autenticar</button>
  <button @click="validate"> Validar si lo hice bien</button>
  <textarea name="resultado" id="123" cols="30" rows="10" v-model="resultado1"></textarea>
</template>

<script>
import axios from 'axios'

export default { 
  name : 'auth',
  
  data() {
    return {
      tokenAutenticacion : null,
      // Autenticación
      resultado1 : null,
      email : "MagosDS2",
      password : "Magos_DS2"
    }
  },

  methods : {
    auth() {

      const auth = axios.create({
        baseURL : 'http://localhost:8000/api'
      })

      auth.post('./auth', {
        "email" : this.email,
        "password" : this.password
      } )
        .then( resp => {
          const {data} = resp
          console.log( data )
          this.resultado1 = JSON.stringify( this.tokenAutenticacion = data.tokens.access )
          console.log( this.tokenAutenticacion)
        })
        .catch( err => console.log( err ))


      //Segundo método  
      // axios({
      //   method: 'POST',
      //   url : 'http://localhost:8000/api/auth',
      //   data : {
      //     email : this.email,
      //     password : this.password
      //   }
      // }).then( resp => {
      //   console.log(this.email)
      //   console.log(this.password)
      //   console.log( resp.data )
      // })


      // Tercer método   

      // const config = {
      //   method: 'post',
      //   url: 'http://localhost:8000/api/auth',
      //   headers: { // Sin el header también funciona
      //     'Content-Type': 'application/json'
      //   },
      //   data : {
      //   "email": this.email,
      //   "password": this.password
      //   }
      // };

      // axios(config)
      // .then( resp => {
      //   console.log(JSON.stringify(resp.data));
      // })
      // .catch( err => {
      //   console.log(err);
      // });

      
    },

    // OJO, este método debe de usarse cuando se haya utenticado
    validate() {
      const auth = axios.create({
        baseURL : 'http://localhost:8000/api',
        headers : {
          Authorization : `Bearer ${this.tokenAutenticacion}`
        }
      })

      auth.get('./auth', {
        email : this.email,
        password : this.password
      })
        .then( resp => {
          console.log( this.tokenAutenticacion)
          console.log(  resp )
          this.resultado1 = JSON.stringify( resp.data )
        })
        .catch( err => console.log( err ))

    }
  }
}
</script>

<style>

</style>