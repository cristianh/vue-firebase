<template lang="html">

  <section class="data-base-fire">
    <h1>data-base-fire Component</h1>
   <div v-show="formedit">
    <form @submit.prevent="Guardar">
      <label for="nombre">Nombre</label>
        <input name="nombre" id="" v-model="nombre" type="text" value=""><br>
        <label for="correo">Correo</label>
        <input name="correo" id="" v-model="correo" type="text" value=""><br>
        <label for="edad">Edad</label>
         <input name="edad" id="" v-model="edad"  type="text" value=""><br>
         <input type="submit" value="Guardar">
      </form>
      </div>
      <br>
      <div >
        <table border="1" class="table"width="100%">
          <thead>
            <tr>
              <th>Nombre</th>
              <th>Correo</th>
              <th>Edad</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(value,key,index) in allusuarios">
              <td>
               <span>{{ value.nombre }}</span>
              </td>
              <td>
                <span>{{ value.correo }}</span>
              </td>
              <td>
               <span>{{ value.edad }}</span>
              </td>
                <td>
               <button  @click="eliminar(value.key)"class="btn">
                 Eliminar
               </button>
               <button  @click="editar(value,value.key)"class="btn">
                 Editar
               </button>
              </td>
              
            </tr>
          </tbody>
        </table>
      <br>
        <div v-show="!formedit">
          <form @submit.prevent="Actualizar">
      <label for="nombre">Nombre</label>
        <input name="nombre" id="" v-model="nombreed" type="text" value=""><br>
        <label for="correo">Correo</label>
        <input name="correo" id="" v-model="correoed" type="text" value=""><br>
        <label for="edad">Edad</label>
         <input name="edad" id="" v-model="edaded"  type="text" value=""><br>
         <input type="submit" value="Guardar">
      </form>
      </div>
    </div>
  {{ keysdb }}
  </section>
</template>

<script lang="js">
 import firebase from 'firebase';
 import axios from 'axios'
  // Initialize Firebase
        var config = {
            apiKey: "AIzaSyC2U4etTnUhTCpwSXp_QAAI1h_TAjjA3gI",
            authDomain: "vue-firebase-91c78.firebaseapp.com",
            databaseURL: "https://vue-firebase-91c78.firebaseio.com",
            projectId: "vue-firebase-91c78",
            storageBucket: "vue-firebase-91c78.appspot.com",
            messagingSenderId: "805534221479"
        };
        firebase.initializeApp(config);
        const db = firebase.database();
      
  export default  {
    name: 'DataBaseFire',
    props: [],
    created() {
     
    },
    mounted() {
        db.ref('/perfiles')
        .on('value',snapshot => this.cargarusuario(snapshot.val()))
    },
    data() {
      return {
        'correo':null,
        'edad':null,
        'nombre':null,
        'correoed':null,
        'edaded':null,
        'keyed':null,
        'nombreed':null,
        'allusuarios':[],
        'userEdit':[],
        'keysdb':null,
        'formedit':true
      }
    },
    methods: {
        Guardar(){
          db.ref('/perfiles/').push({
            'nombre': this.nombre,
            'correo': this.correo,
            'edad': this.edad
        }).then(function (data) {
          
        });},
        cargarusuario(usuarios) {
         
          this.allusuarios = []
          for (let key in usuarios){
            this.allusuarios.push({
              nombre:usuarios[key].nombre,
              correo:usuarios[key].correo,
              edad:usuarios[key].edad,
              key:key
            });
          }
        },
        editarUsuario(mensaje,usuario,estado,key){
          switch (estado) {
            case 'correo':
                 db.ref('/perfiles/'+key).update({
              correo:mensaje.target.innerHTML
            }).then(function (data) {})
              break;
            case 'edad':
                 db.ref('/perfiles/'+key).update({
              edad:mensaje.target.innerHTML
            }).then(function (data) {})
              break;
          case 'nombre':
                 db.ref('/perfiles/'+key).update({
              nombre:mensaje.target.innerHTML
            }).then(function (data) {})
              break;
            default:
              break;
          }
        },
        eliminar(key){
          console.log(key);
          if(confirm('Seguro?')){
          db.ref('/perfiles/'+key).remove();  
          } 
        },
        editar(user,key){
          this.formedit=false;
          this.correoed=user.correo;
          this.edaded=user.edad;
          this.nombreed=user.nombre;
          this.keyed=key;
        },
        Actualizar(){
           db.ref('/perfiles/'+this.keyed).update({
            'nombre': this.nombreed,
            'correo': this.correoed,
            'edad': this.edaded
        }).then(function (data) {
         
        });
         this.formedit=true;
         this.limpiarcampos();
        },
        limpiarcampos(){
          this.nombreed=null;
          this.correoed=null;
          this.edaded=null;
          this.nombre=null;
          this.correo=null;
          this.edad=null;
          this.keyed=null;
        }
    },
    computed: {
        
    }
}
</script>

<style scoped lang="less">
  .data-base-fire {

  }
</style>
