<template>
  <div id="app">
    <div class="container h-80">
    <div class="row align-items-center h-100">
    <div class="col-3 mx-auto">

        <div class="text-center">
            <img id="profile-img" class="rounded-circle profile-img-card" src="https://i.postimg.cc/t4zcszj6/cap.png" />
            <p id="profile-name" class="profile-name-card"></p>
            
            <!-- Formulario -->
            <form v-on:submit.prevent="login" class="form-signin">
                
                <!-- Inputs para ingresar nombre y contraseña -->
                
                <input type="usuario" 
                  name="usuario" 
                  id="inputUser" 
                  class="form-control form-group" 
                  placeholder="usuario"
                  v-model="usuario" 
                  required autofocus>
                
                <input type="password" 
                  name="password" 
                  id="inputPassword" 
                  class="form-control form-group" 
                  placeholder="password"
                  v-model="password" 
                  required autofocus>

                <!-- Boton ingresar -->
                <b-button
                  type="submit">
                  ENTER
                </b-button>

                <b-alert show variant="danger" v-if="error">
                  {{error_msg}}
                </b-alert>
            
            </form>
        </div>
    </div>
    </div>
    </div>
  <router-view/>
  </div>
</template>

<script>

// Importamos axios
import axios from 'axios';

export default {
  name: 'Home',
  components: {

  },

  // Ponemos la lógica del login
  data: function(){
    return{
      usuario: "",
      password: "",
      error: false,
      error_msg: "Usuario incorrecto",
    }
  },
  methods:{
    login(){
      // Para verlo por consola
      // console.log(this.user);
      // console.log(this.password);
    
      // Productivo, la estructura es así
      // Toma los usuarios
      const json = {
        "usuario" : this.usuario,
        "password": this.password
      };
      // Y los autentica con axios
      // La función de axios pide una URL y un json(o body)
      axios.post('https://api.solodata.es/auth', json)
      .then(data =>{
        if(data.data.status == "ok"){
          localStorage.token = data.data.result.token;
          this.$router.push("dashboard");
        }else{
          this.error = true;
          this.error_msg = data.data.result.error_msg;
          console.log(data);
        }
      })
      
    }
  }

}
</script>

<style>

body,html {
    background-image: url('https://i.imgur.com/xhiRfL6.jpg');
    height: 100%;
}

#profile-img {
    height:180px;
}

#inputUser{
  margin-bottom: 2rem;
}

#inputPassword{
  margin-bottom: 2rem;
}

.h-80 {
    height: 80% !important;
}

/* .btnLogin{
  width: 15rem;
  background: rgb(1, 2, 10);
  color: rgb(255, 255, 255);
  border-radius: 5%;
  border-color: none;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  font-weight: bold;
} */

</style>
