<template>
  
  <h2> {{tituloBonito}} </h2>

  <div class="separador">
    <input type="text" placeholder="Nombre" v-model="nombrecito">
    <input type="text" placeholder="Trabajo" v-model="trabajito">
    <button @click="createUser(nombrecito,trabajito)"> Registrar </button> 
    <br>
  </div>
  


  <div class="separador">
    <input type="number" placeholder="Inserte la página de usuarios que quiere ver" v-model="paginaXD"> <!-- Nótese que se usa el v-model, y se registra en data para que así la variable pueda ser guardad y mutar sin problema -->
    <br>
      <button @click="getListUsers">Imprimir lista usuarios</button>
      <template v-if="respuestaJson"> 
        <div v-for=" persona in respuestaJson" :key="persona.id">
          Nombre: {{persona.first_name}} <br>
          Apellido: {{ persona.last_name }} <br> 
          Email: {{ persona.email }} <br> 
          Imagen: <img :src="persona.avatar" :alt="'imagen'+(persona.id)">  
        </div> 
      </template>
  </div>
  
  
  
  <br>
  <h6> Resultado JSON después de hacer las peticiones HTTP (Spoiler, se ven horribles)</h6>
  <textarea name="" id="" cols="30" rows="30" readonly v-model="contenido">  </textarea>
  <br>



  <div class="separador">
    <span> Oprime el botón para generar un gif aleatorio: </span>
    <button @click="getgifsRandoms()">Botón gifAleatorio</button>
    <br>
    <img :src="urlImg" alt="👀urlxd" v-if="urlImg"> <!-- Los : (o v-on) son vitales porque así estás obligando que el parámetro que se lea, sea como si fuera JavaScript, los v-if y demás no lo tienen porque ya lo tienen por defecto-->
  </div> 
  
</template>
  





<script>
import axios from 'axios'
export default {
  name : 'entrada_simple',

  props : {
    'tituloBonito' : 'titulo por defecto'
  },

  data() {
    return {
      contenido : 'Mensaje por defecto',
      urlImg : null,
      reqres : 'https://reqres.in/api', // Se establece esta variable para hacer los llamados con axios más sencillos, y no tener que estar escribiendo todo eso
      paginaXD : null,
      respuestaJson : null,
      nombrecito : "", // Pongo esto para que Vue no llore con v-models sin referenciar
      trabajito : "" // Pongo esto para que Vue no llore con v-models sin referenciar
    }
  },

  methods : {
    createUser (nombre, trabajo) {
      const crearUsuario = axios.create( {
        baseURL : this.reqres 
      })

      crearUsuario.post( './users',
        {
          // Este es el body
          name: nombre,
          job: trabajo
        }).then( resp => {
          console.log( resp.data )
          this.contenido = JSON.stringify( resp.data ) 
        })

    },

    // Método que hace un ejemplo simple de petición HTTP en la página https://reqres.in/ 
    getListUsers() {
      // Url original: https://reqres.in/api/users?page={CUALQUIERNUMERO!}
      const getListUsers = axios.create({
        baseURL : this.reqres,
        params : {
          page : this.paginaXD // Note que se unió con la variable tejida en la página web, por medio de v-model
        }
      })

      getListUsers.get('./users').then( resp => {
        // De ser necesario, imprime resp para ver todo el JSON que se devuelve
        //console.log( resp.data.data )
        const { data } = resp.data
        this.contenido = JSON.stringify( data )
        this.respuestaJson = data
      })
      
    },
    
    getgifsRandoms( ) {
      // URL original : https://api.giphy.com/v1/gifs/random?api_key=YIWqNq2xpTioo8SYStdM0t3PjdYeeT2o

      const API_KEY = 'YIWqNq2xpTioo8SYStdM0t3PjdYeeT2o' // Un API key que abrí con una cuenta, es gratis

      const apiGiphy = axios.create({
        baseURL : 'https://api.giphy.com/v1/gifs',
        params : {
          api_key : API_KEY
        }
      })

      apiGiphy.get('./random')
        .then( resp => {
          //console.log( resp.data.data.images.original.url) // Este sería el link del url
          const { data } = resp.data // Apliqué desestructuración, lo mismo que imprime arriba
          const { url } = data.images.original
          this.urlImg = url 
          this.contenido = JSON.stringify( resp ) // Para convertirlo en string
        })

    }
    
  }
}
</script>





<style scoped>
  textarea {
    resize: none;
    width: 70%;
    height: 350px;

  }

  img {
    padding: 30px;
    width: 300px;
    height: 300px;
  }
  
  .separador {
    border: 1px black solid;
    padding: 10px;
    margin: 10px;
    background-color: gray;
  }

</style>