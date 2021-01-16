<template>
  <div>
    <div>
      <h1>Ingresar un animal</h1>
      <label for="nombrevulgar" align="left">Nombre vulgar:</label>
      <input id="nombrevulgar" v-model="nombrevulgar" align="left"/>
      <a style="color: #ffffff">espacio en blancoooooooo </a>
      <br />
      <label for="nombrecientifico">Nombre cientifico:</label>
      <input type="text" id="nombrecientifico" v-model="nombrecientifico" />
      <a style="color: #ffffff">espacio en blanco </a>
      <button name="guardar" v-on:click="guardar()">Guardar</button>
      <br />
      <label for="familia">Familia:</label>
      <input type="text" id="familia" v-model="familia" />
      <a style="color: #ffffff">espacio en blancoo </a>
      <br />
      <label for="extincion">Extincion:</label>
      <input type="checkbox" id="extincion" v-model="extincion" />
      <a style="color: #ffffff">espacio en blancooooo </a>
      <br />
      <br />
    </div>
    <div class="table-responsive">
      <table class="table" WIDTH="20%">
        <thead>
          <tr>
            <th WIDTH="50">ID</th>
            <th WIDTH="150">Nombre vulgar</th>
            <th WIDTH="150">Nombre cientifico</th>
            <th WIDTH="150">Familia</th>
            <th WIDTH="150">Extinción</th>
            <th WIDTH="50">Editar</th>
            <th WIDTH="50">eliminar</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="todo in todos" :key="todo.id">
            <td>{{ todo.id }}</td>
            <td>{{ todo.nombrevulgar }}</td>
            <td>{{ todo.nombrecientifico }}</td>
            <td>{{ todo.familia }}</td>
            <td>{{ todo.extincion }}</td>
            <td><button name="editar" v-on:click="editar(todo.id, todo.nombrevulgar, todo.nombrecientifico, todo.familia, todo.extincion)" >Editar</button></td>
            <td>
              <button name="eliminar" v-on:click="eliminar(todo.id)">
                Eliminar
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <br/>
    <div >
      <h1>Editar un animal</h1>
      <label for="nombrevulgaredit">Nombre vulgar:</label>
      <input id="nombrevulgaredit" v-model="nombrevulgaredit" />
      <a style="color: #ffffff">espacio en blancoooooo </a>
      <br />
      <label for="nombrecientificoedit">Nombre cientifico:</label>
      <input type="text" id="nombrecientificoedit" v-model="nombrecientificoedit" />
      <a style="color: #ffffff">espacio en blanco </a>
      <button id="editar" name="editar" v-on:click="editarGuardar()">Editar</button>
      <br />
      <label for="familiaedit">Familia:</label>
      <input type="text" id="familiaedit" v-model="familiaedit" />
      <a style="color: #ffffff">espacio en blanc </a>
      <br />
      <label for="extincionedit">Extincion:</label>
      <input type="checkbox" id="extincionedit" v-model="extincionedit" />
      <br />
      
    </div>
  </div>
</template>

<script>
import axios from "axios";


export default {
  name: "HelloWorld",
  props: {
    msg: String,
    nombrevulgar: String,
    nombrecientifico:String,
    familia:String,
    extincion:Boolean,
    nombrevulgaredit: String,
    nombrecientificoedit:String,
    familiaedit:String,
    extincionedit:Boolean,
    idedit:Number
  },
  data() {
    return {
      todos: null,
    };
  },
  mounted() {
    this.getTodos();
  },
  methods: {
    getTodos() {
      axios
        .get("http://127.0.0.1:8000/api/animals")
        .then((response) => {
          this.todos = response.data;
        })
        .catch((e) => console.log(e));
    },
    eliminar(id) {
      axios
        .delete("http://127.0.0.1:8000/api/animals/" + id)
        .then()
        .catch((e) => console.log(e));
        setTimeout(() => {
          location.reload();
        }, 300);
        
    },
    guardar() {
      axios.post("http://127.0.0.1:8000/api/animals/",{
        nombrevulgar: this.nombrevulgar,
        nombrecientifico: this.nombrecientifico,
        familia:this.familia,
        extincion:this.extincion
        }).then().catch(e=>console.log(e));
        
        this.nombrevulgar =""
        this.nombrecientifico =""
        this.familia=""
        this.extincion=false
        setTimeout(() => {
          location.reload();
        }, 300);
    },
    editar(id, nombrev, nombrec, familia, extincion){
      this.idedit= id
      this.nombrevulgaredit = nombrev
      this.nombrecientificoedit = nombrec
      this.familiaedit = familia
      this.extincionedit = extincion
    },
    editarGuardar(){
      axios.put("http://127.0.0.1:8000/api/animals/"+this.idedit,{
      nombrevulgar:this.nombrevulgaredit,
      nombrecientifico:this.nombrecientificoedit,
      familia:this.familiaedit,
      extincion:this.extincionedit
      }).then().catch(e=>console.log(e));

       this.idedit= ""
      this.nombrevulgaredit = ""
      this.nombrecientificoedit = ""
      this.familiaedit = ""
      this.extincionedit = ""
      location.reload();
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
