<template>
  <div class="main">
    <h1 style="font-size: 2.3rem">Gestor de <span style="color: var(--enfasis)">Tareas</span></h1>
    <div class="app">
      <hr>
      <div :class="{ 'editardiv': editar, 'noeditardiv': !editar }" id="editar">
        <button @click="cerrarEditar()"
          style="width: 22px; height: 25px; margin-left:410px; margin-top: 20px;transition: 0s;">X</button>
        <h1 style="margin-top: 10px; text-align: center;">Editar tarea</h1>
        <input type=" text" name="" id="editarnombre" placeholder="Nuevo nombre" class="input"
          style="width: 400px; margin-top: 30px">
        <input type="text" name="" id="editartarea" placeholder="Edita tarea" class="input"
          style="width: 400px; margin-top: 30px">
        <button @click="editarTarea()" style="margin-top: 30px; transition: 0s;">Editar</button>
      </div>
      <div class="inputs">
        <input type="text" name="Escribe la tarea" id="tarea" placeholder="Introduce el nombre " class="input">
        <input type="text" name="" id="fecha" placeholder="Introduce la tarea" class="input">
      </div>
      <div class="botones">
        <button @click="nuevaTarea()" class="submit">Añadir</button>
        <button @click="borrarTodas()" class="borrartodo">Borrar Todo</button>
      </div>
      <p style="color: var(--borrar)" v-if="vacio">¡¡Algun campo vacio!!</p>
      <p style="color: var(--borrar)" v-if="formato">¡¡Caracteres no permitidos</p>
      <h1 v-if="tareas.length == 0" style="font-size: 1.5rem; margin-top: 30px;transition: .7s">No hay tareas
        pendientes
      </h1>
      <h1 v-else="tareas.length != 0" style="font-size: 1.5rem; margin-top: 30px;">Tienes <span
          style="color: var(--enfasis);">{{
          tareas.length
          }}</span> tareas pendientes</h1>
      <hr>
      <div class="uldiv" :class="{ 'dark': theme, '': !theme }">
        <ul>
          <li v-for="item, n in tareas" :key="n">
            <p :class="{ 'marcado': item.check, '': !item.check }">
              <span style="color: var(--enfasis); font-weight: bold;"><input class="inputc" type="checkbox"
                  v-model="item.check">
              </span>
              <span style="color: var(--enfasis); font-weight: bold;"> Nombre: </span>
              {{
              item.tarea
              }} |
              <span style="color: var(--enfasis); font-weight: bold;"> Tarea: </span> {{
              item.fecha
              }}
              <button @click="visualizarEditar(item)" class="editar" style="width: 55px;">Editar</button>
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
      index: 0,
      vacio: false,
      formato: false,
      theme: false,
      editar: false,
      item: []
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
      let x = this.i
      elemento.index = x;
      this.i++
      document.getElementById('tarea').value = ''
      document.getElementById('fecha').value = ''
      this.tareas.push(elemento)
      console.log(this.tareas)
      localStorage.setItem('datos', JSON.stringify(this.tareas));
      localStorage.setItem('index', JSON.stringify(this.i));
    },
    borrarTodas() {
      this.tareas = []
      this.i = 0
      document.getElementById('tarea').value = ''
      document.getElementById('fecha').value = ''
      this.vacio = false;
      this.formato = false;
      localStorage.clear();
      for (let i=0; i<this.tareas.length; i++) {
      this.tareas[i].index = i;
    }
    },
    borrarItem(item) {
      this.tareas = this.tareas.filter((t) => t !== item)
      localStorage.clear();
      localStorage.setItem('datos', JSON.stringify(this.tareas));
      for (let i=0; i<this.tareas.length; i++) {
      this.tareas[i].index = i;
    }
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
    },
    cambiarTema() {
      this.theme = !this.theme;
    },
    visualizarEditar(item) {
      this.index = item.index
      console.log(this.index)
      this.editar = true;
    },
    editarTarea() {
      let n = document.getElementById('editarnombre').value;
      let e = document.getElementById('editartarea').value;
      let d = document.getElementById('editar');
      if (n === '' && e === '') {
        this.editar = false;
      }
      else if (n === '' && e !== '') {
        this.tareas[this.index].fecha = e;
      }
      else if (n !== '' && e === '') {
        this.tareas[this.index].tarea = n;
      }
      else if (n !== '' && e !== '') {
        this.tareas[this.index].fecha = e;
        this.tareas[this.index].tarea = n;
      }
      localStorage.setItem('datos', JSON.stringify(this.tareas));
      this.editar = false;
      document.getElementById('editarnombre').value = '';
      document.getElementById('editartarea').value = '';
    },
    cerrarEditar() {
      this.editar = false;
    }
  },
  created: function () {
    let datos = JSON.parse(localStorage.getItem('datos'));
    if (datos === null) {
      this.tareas = [];
    }
    else {
      this.tareas = datos;
      this.i = this.tareas.length;
    }
    for (let i=0; i<this.tareas.length; i++) {
      this.tareas[i].index = i;
    }
  }
}
</script>

<style scoped>
h1,
ul {
  margin-top: 10px;
  color: var(--color);
}


li {
  margin-top: 30px;
  font-size: 1.1rem;
  text-align: center;
  background-color: var(--background2);
  border-radius: 5px;
  width: 600px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.editardiv {
  width: 500px;
  height: 600px;
  position: fixed;
  background-color: var(--background);
  border: 1px solid var(--background2);
  border-radius: 5px;
  top: 100px;
  color: var(--color);
  margin: auto;
  display: flex;
  align-items: center;
  flex-direction: column;
  transition: .7s;
}

.noeditardiv {
  visibility: hidden;
  width: 0px;
  height: 0px;
  top: -300px;
}

.editar2 {
  display: flex;
  justify-content: center;
  align-items: center;
}

.dark {
  background-color: var(--backlight);
  color: var(--colorlight);
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

.editartxt {
  color: var(--color);
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
    width: 400px;
  }
}

@media (max-width: 411px) {
  .input {
    width: 300px;
  }
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