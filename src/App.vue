<template>
  <div id="app">
    <main class="container">
      <section id="area-form" class="back_branco">
        <div id="area-input">
          <h1 title=" mural" id="mural">MURAL</h1>

          <label for="user">Usuário</label>
          <input type="text" name="user" id="user" v-model="name" />
          <label for="message">Digite Seu Texto</label>
          <textarea
            name="input-message"
            id="input-message"
            cols="21"
            rows="10"
            v-model="message"
          >
          </textarea>
          <button class="btn btn-enviar" v-on:click="enviar()">Enviar</button>
        </div>
      </section>

      <section id="area-post" class="back_branco">
        <article class="card" v-for="(dado, index) in dados" :key="index">
          <h2 id="output_user_name">{{ dado.name }}</h2>
          <p id="output-text">{{ dado.message }}</p>
          <a href="#" v-on:click="excluir(dado.id)">Excluir</a>
          <a href="#" v-on:click="editar(dado)">Editar</a>
        </article>
      </section>
    </main>
  </div>
</template>

<script lang="ts">
import axios from "axios";
interface IResponse {
  _id: string;
  name: string;
  message: string;
}
export default {
  name: "app",
  data: () => {
    return {
      dados: [],
      id: 0,
      name: "",
      message: "",
      update: false,
    };
  },

  methods: {
    buscar() {
      axios
        .create({
          baseURL: "http://100.64.144.245:5000/",
          timeout: 10000,
        })
        .get("/mural")
        .then((response) => {
          console.log(response);
          this.dados = response.data.mural.map((element: IResponse) => {
            return {
              id: element._id,
              name: element.name,
              message: element.message,
            };
          });
        })
        .catch((error) => {
          alert(error);
        });
    },

    excluir(id) {
      axios
        .create({
          baseURL: "http://100.64.144.245:5000/",
          timeout: 10000,
        })
        .delete("/mural", { data: { id: id } })
        .then((response) => {
          console.log(response);
          this.buscar();
        })
        .catch((error) => {
          alert(error);
        });
    },

    enviar() {
      if (!this.update) {
        axios
          .create({
            baseURL: "http://100.64.144.245:5000/",
            timeout: 10000,
          })
          .post("/mural", { name: this.name, message: this.message })
          .then((response) => {
            console.log(response);
            this.buscar();
          })
          .catch((error) => {
            alert(error);
          });
      } else {
        this.atualizar();
      }
    },

    editar(dado) {
      this.id = dado.id;
      this.name = dado.name;
      this.message = dado.message;
      this.update = true;
    },

    atualizar() {
      axios
        .create({
          baseURL: "http://100.64.144.245:5000/",
          timeout: 10000,
        })
        .put("/mural", { id: this.id, message: this.message })
        .then((response) => {
          console.log(response);
          this.buscar();
          this.update = false;
          (this.name = ""), (this.message = "");
        })

        .catch((error) => {
          alert(error);
        });
    },
  },
  created() {
    this.buscar();
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
html {
  font-size: 95%;
}

body {
  font-family: "Roboto", Arial, sans-serif;
  background: linear-gradient(
      182deg,
      rgba(133, 157, 199, 0.5) 102.27%,
      rgba(255, 255, 255, 0) 103.89%
    ),
    linear-gradient(178.79deg, #9fbced 101.7%, rgba(77, 103, 242, 0) 107.55%),
    #35465e;
}
.container {
  position: relative;
  top: 100px;
  height: 100%;
  width: 90%;
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 800px;
}
h1 {
  font-size: 3rem;
}
.back_branco {
  background-color: #f4f4f4;
  margin-left: 3rem;
}
/*Header ####################################*/
header {
  display: inline-block;
  float: right;
  margin: 2rem 4rem 0;
}

header input {
  border-radius: 20px;
  border-style: none;
  padding: 0.4rem 0;
}
/*AREA FORM*/
section {
  box-shadow: 3px 5px 5px rgb(0, 0, 0, 0.2);
}
section#area-form {
  display: block;
  width: 30%;
  padding: 2rem;
}
section#area-form input,
label,
textarea {
  display: block;
}

section#area-form input,
textarea {
  background-color: #dcdcdc;
  border: none;
  width: 100%;

  padding: 0.2rem 0;
  font-size: 2rem;
}

label {
  font-size: 1.5rem;
  padding: 1rem 0 1rem;
}

.btn {
  font-size: 1.5rem;
  width: 50%;
  padding: 0.5rem 0;
  margin-top: 1rem;
  background: #dcdcdc;
  border: none;
}
/*AREA post*/

section#area-post {
  width: 65%;
  padding: 2.5rem;
  height: 420px;
  overflow-y: auto;
}

.card {
  border: 3px solid rgb(143, 182, 219);
  padding: 2rem;
  margin-bottom: 2rem;
  border-radius: 10px;
}
.card p {
  margin-top: 1rem;
}
</style>
