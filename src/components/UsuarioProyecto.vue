import axios from 'axios';
<template>
  <h1> Registro, validación y lectura de Usuario</h1>
  <h6><b>Nota: </b>Las casillas en verde son aquellas donde está permitido modificar la información (los demás son valores redundantes de los principales)</h6>
  <input type="text" placeholder="Email" v-model="email" class="permitidoModificar">
  <input type="text" placeholder="Password" v-model="password" class="permitidoModificar">
  <button @click="auth">Autenticar</button>
  <button @click="validate"> Validar si lo hice bien</button>
  <button @click="getUsers">Obtener lista usuarios</button>
  <input type="number" placeholder="Id usuario" v-model="idUsuarioSearch" class="permitidoModificar">
  <button @click="getUser">Obtener Usuario</button>
  <textarea name="resultado" id="123" cols="30" rows="10" v-model="resultado1" readonly></textarea>

  <hr>
  <h1>Registro y modificación de Usuario</h1>
  <div>
    <input type="number" placeholder="Id Usuario" :value="idUsuarioSearch" readonly> 
    <input type="text" placeholder="username" readonly onmousedown="return false" :value="email" >
    <input type="text" placeholder="nombre" v-model="nombre" class="permitidoModificar">
    <input type="text" placeholder="apellido" v-model="apellido" class="permitidoModificar"> 
    <input type="email" placeholder="email" readonly onmousedown="return false" :value="email">   
    <input type="password" placeholder="Password" readonly onmousedown="return false" :value="password">  
    <input type="text" placeholder="documento" v-model="documento" class="permitidoModificar">   
    <select name="Rol" id="rol" v-model="rol" class="permitidoModificar"> 
      <option value="asociado">Asociado</option>
      <option value="admin">Administrador</option>
      <option value="cliente">Cliente</option>
    </select> 
    <input type="date" v-model="fechaNacimiento" class="permitidoModificar">
  </div>
  <button @click="createUser">Crear usuario</button>
  <button @click="modifyUser">Modificar usuario</button>

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

      // Atributos de usuario
      email : "magosds2@correounivalle.edu.co",
      password : "Magos_DS2",
      nombre : "",
      apellido : "",
      //username : this.email, // se obvia porque se repite con el email, es temporal
      documento : "",
      rol : "",
      idUsuarioSearch : "",
      fechaNacimiento : "",

    }
  },

  methods : {
    auth() {

      const auth = axios.create({
        baseURL : 'http://localhost:8000/api'
      })

      auth.post('./login/', {
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
        baseURL : 'http://localhost:8080/api',
        headers : {
          Authorization : `Bearer ${this.tokenAutenticacion}` 
        }
      })

      auth.get('./login', {
        email : this.email,
        password : this.password
      })
        .then( resp => {
          console.log( this.tokenAutenticacion)
          console.log(  resp.data.user )
          this.resultado1 = JSON.stringify( resp.data )
        })
        .catch( err => console.log( err ))




      /** --------  */

      // var axios = require('axios');
      // var data = JSON.stringify({
      //   "email": "MagosDS2",
      //   "password": "Magos_DS2"
      // });

      // var config = {
      //   method: 'get',
      //   url: 'http://localhost:8080/api/login',
      //   headers: { 
      //     'Authorization': `Bearer ${this.tokenAutenticacion}` , 
      //     'Content-Type': 'application/json'
      //   },
      //   data : data
      // };

      // axios(config)
      // .then(function (response) {
      //   console.log(JSON.stringify(response.data));
      // })
      // .catch(function (error) {
      //   console.log(error);
      // });
    },

    getUsers() {
      const getUsers = axios.create({
        baseURL : 'http://localhost:8080/api',
        headers : {
          Authorization : `Bearer ${ this.tokenAutenticacion }`
        }
      })

      getUsers.get('./users/')
        .then( resp => console.log( resp.data ))
        .catch( err => console.log( err ))
    },
    
    getUser() {
      const getUser = axios.create({
        baseURL : 'http://localhost:8080/api',
        headers : {
          Authorization : `Bearer ${ this.tokenAutenticacion }`
        }
      })  
      console.log( this.idUsuarioSearch)
      getUser.get(`./users/${this.idUsuarioSearch}`)
        .then( resp => console.log( resp ))

 

      // Solución de Postman, tampoco funciona
      // const config = {
      //   method: 'get',
      //   url: 'http://localhost:8000/api/usuarios/2',
      //   headers: { 
      //     'Authorization': `Bearer ${ this.tokenAutenticacion }`,
      //     "Access-Control-Allow-Origin": "*"
      //   }, 
      // };

      // axios(config)
      // .then(function (response) {
      //   console.log(JSON.stringify(response.data));
      // })
      // .catch(function (error) {
      //   console.log(error);
      // });


    },

    createUser() {
      const createUser = axios.create({
        baseURL : 'http://localhost:8080/api',
        headers : {
          Authorization : `Bearer ${ this.tokenAutenticacion }`
        }
      })

      createUser.post('./users/', {
        "username": this.email,
        "first_name": this.nombre,
        "last_name": this.apellido,
        "email": this.email,
        "rol": this.rol,
        "password": this.password,
        "fechaNacimiento": this.fechaNacimiento,
        "documento" : this.documento
      })
        .then( resp => console.log( resp.data )
        )
        .catch( err => console.log( err ))

    },

    // Para modificar el usuario, se necesitan 5 atributos del usuario:
    // username
    // email
    // password
    // rol
    // documento
    modifyUser() {
      // const modifyUser = axios.create({
      //   baseURL: 'http://localhost:8080/api/users',
      //   headers : {
      //     Authorization: `Bearer ${ this.tokenAutenticacion }`
      //   }
      // })
      const apellido = (this.apellido.trim()) ? `"last_name" : "${this.apellido}",` : ""
      const nombre = (this.nombre.trim()) ? `"first_name" : "${this.nombre}",` : ""
      const id = (this.idUsuarioSearch) ? `"id" : ${this.idUsuarioSearch},` : "" 
      const fecha = (this.fechaNacimiento.trim()) ? `"fechaNacimiento" : "${ this.fechaNacimiento}",` : ""
      let jsonCambios = `{` +
        apellido +
        nombre +
        id +
        fecha  +
        `"username" : "${this.email}",` +
        `"email" : "${this.email}",` +
        `"password" : "${this.password}",` +
        `"rol" : "${this.rol}" ,` +
        `"documento" : "${ this.documento}"` +
        "}" 

      console.log( jsonCambios)
      // console.log( JSON.parse(jsonCambios) )

    //   modifyUser.put(`./modify/${this.idUsuarioSearch}`, jsonCambios)
    //     .then( resp => console.log( resp.data ))
    //     .catch( err => console.log( err ))
    // }

      // Código sacado de Postman
      const configuracion = {
        method: 'put',
        url: `http://localhost:8080/api/users/modify/${this.idUsuarioSearch}`,
        headers: { 
          'Authorization': `Bearer ${this.tokenAutenticacion}`, 
          'Content-Type': 'application/json'
        },
        data : jsonCambios
      }

      axios( configuracion )
        .then( resp => console.log( resp ))
        .catch( err => console.log( err))


    }
  }
}
</script>

<style scoped>
  .permitidoModificar {
    background-color: greenyellow;
    color: #401212;
    font-weight: 550;
  }
</style>