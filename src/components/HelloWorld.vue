<template>
  <div class="main">
    <h1 style="font-size: 2.3rem">Gestor de <span style="color: var(--enfasis)">Tareas</span></h1>
    <div class="app">
      <hr>
      <div class="inputs">
        <input type="text" name="Escribe la tarea" id="tarea" placeholder="Introduce tu tarea " class="input">
        <input type="text" name="" id="fecha" placeholder="Introduce la fecha" class="input">
      </div>
      <div class="botones">
        <button @click="nuevaTarea()" class="submit">Añadir</button>
        <button @click="borrarTodas()" class="borrartodo">Borrar Todo</button>
      </div>
      <p style="color: var(--borrar)" v-if="vacio">¡¡Algun campo vacio!!</p>
      <p style="color: var(--borrar)" v-if="formato">¡¡Caracteres no permitidos</p>
      <h1 v-if="tareas.length == 0" style="font-size: 1.5rem; margin-top: 30px;transition: .7s">No hay tareas pendientes
      </h1>
      <h1 v-else="tareas.length != 0" style="font-size: 1.5rem; margin-top: 30px;">Tienes <span
          style="color: var(--enfasis);">{{
          tareas.length
          }}</span> tareas pendientes</h1>
      <hr>
      <div class="uldiv">
        <ul>
          <li v-for="item, n in tareas" :key="n">
            <p :class="{ 'marcado': item.check, '': !item.check }">
              <span style="color: var(--enfasis); font-weight: bold;"><input class="inputc" type="checkbox"
                  v-model="item.check">
              </span>
              {{
              item.tarea
              }} |
              <span style="color: var(--enfasis); font-weight: bold;"> Para: </span> {{
              item.fecha
              }}
              <button @click="borrarItem(item)" class="button">X</button>
            </p>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tareas: [
      ],
      i: 0,
      vacio: false,
      formato: false,
    }
  },
  methods: {
    nuevaTarea() {
      let tarea = document.getElementById('tarea').value
      let fecha = document.getElementById('fecha').value
      if (tarea === '' && fecha === '') {
        this.vacio = true;
        return true;
      }
      if (tarea.includes("<") || tarea.includes(">") || fecha.includes("<") || fecha.includes(">")) {
        this.formato = true;
        return true;
      }
      let elemento = {}
      let check = false;
      elemento.tarea = tarea
      elemento.fecha = fecha
      elemento.check = check
      this.vacio = false;
      this.formato = false;
      document.getElementById('tarea').value = ''
      document.getElementById('fecha').value = ''
      this.tareas.push(elemento)
      console.log(this.tareas)
      this.i++
      localStorage.setItem('datos', JSON.stringify(this.tareas));
    },
    borrarTodas() {
      this.tareas = []
      document.getElementById('tarea').value = ''
      document.getElementById('fecha').value = ''
      this.vacio = false;
      this.formato = false;
    },
    borrarItem(item) {
      this.tareas = this.tareas.filter((t) => t !== item)
    },
    validar(tarea, fecha) {
      if (tarea === '' && fecha === '') {
        this.ok = true;
        return true;
      }
      if (tarea.includes("<") || tarea.includes(">") || fecha.includes("<") || fecha.includes(">")) {
        this.formato = true;
        return true;
      }
      return false;
    }
  },
  created: function () {
    let datos = JSON.parse(localStorage.getItem('datos'));
    if (datos === null) {
      this.tareas = [];
    }
    else {
      this.tareas = datos;
    }
  }
}
</script>

<style scoped>
h1,
p,
ul {
  margin-top: 10px;
  color: var(--color);
}

li {
  margin-top: 30px;
  font-size: 1.1rem;
  text-align: center;
}

.marcado {
  text-decoration: line-through;
}

.input {
  background-color: var(--background2);
  border: none;
  margin-top: 10px;
  margin-bottom: 20px;
  height: 40px;
  width: 600px;
  font-size: 1rem;
  outline: none;
  text-indent: 20px;
  color: var(--color);
  border-radius: 5px;
}

.inputc {
  background-color: var(--background2);
  outline: none;
  margin-right: 10px;
  border: none;
}

.inputc:focus {
  border: none;
  background-color: var(--background2);
  outline: none;
}

.inputc:checked {
  outline: none;
  color: var(--color);
  background-color: var(--enfasis);
}

ul {
  list-style: none;
  display: table-row;
}

input:focus {
  outline: 2px solid var(--enfasis);
}

.main {
  display: flex;
  align-items: center;
  flex-direction: column;
}

.borrartodo {
  border: 2px solid var(--borrar);
}

.borrartodo:hover {
  background-color: var(--borrar);
}

.botones {
  display: flex;
  justify-content: center;
  flex-direction: row;
}

.button {
  background-color: var(--background);
  border-radius: 5px;
  border: 2px solid var(--borrar);
  width: 25px;
  margin-left: 10px;
  height: 25px;
}

.button:hover {
  background-color: var(--borrar);
}

button {
  background-color: var(--background);
  border-radius: 5px;
  margin-left: 20px;
  border: 2px solid var(--enfasis);
  height: 30px;
  width: 95px;
  color: var(--color);
  transition: .4s;
}

button:hover {
  background-color: var(--enfasis);
  color: var(--background);
  transition: .4s;
}

.inputs {
  display: flex;
  flex-direction: column;
}

hr {
  width: 600px;
}

.app {
  display: flex;
  align-items: center;
  flex-direction: column;
}

@media (max-width: 635px) {
  .input {
  width: 400px;}
}

@media (max-width: 411px) {
  .input {
  width: 300px;}
}

@media (max-width: 311px) {
  .input {
  width: 200px;
  }
  h1 {
    font-size: 1rem;
  }
}
</style>