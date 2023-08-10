<template>
  <div class="containerTareas">
    <div class="tabla">
      <div class="tablaHeader">
        <h3>Pendiente</h3>
        <Plus @click="mostrarInput(1)" class="iconoClickable" />

      </div>
      <hr style="margin-block: 5px;">
      <draggable class="grupoTareas" :list="tablaPendiente" group="tareas" itemKey="nombre" :empty-insert-threshhold="500"
        @change="actualizarLocalStorage">
        <template #item="{ element, index }">
          <div class="itemTarea">
            <div class="descripcionTarea" @click="abrirModalEditar(element, index)">
              <div class="tareaTitulo">
                {{ element.nombre }}
              </div>
              <div class="tareaDescripcion" v-if="element.descripcion">
                {{ element.descripcion }}
              </div>
              <div class="tareaDescripcion" v-if="element.fecha">
                {{ formatearFecha(element.fecha) }}
              </div>
            </div>
            <div>
              <Close @click="eliminarTarea(index, tablaPendiente, 'tablaPendiente')" class="iconoClickable" />
            </div>
          </div>
        </template>
      </draggable>

      <div v-if="mostrarInputPendiente" class="itemTarea itemTareaShadow" style="display:flex; margin-top:5px;">
        <input v-model="nombreTareaPendiente" @keyup.enter="agregarTarea(nombreTareaPendiente, tablaPendiente)"
          class="inputTarea" />
        <Check @click="agregarTarea(nombreTareaPendiente, tablaPendiente, 'tablaPendiente')" class="iconoClickable" />
      </div>
    </div>

    <div class="tabla">
      <div class="tablaHeader">
        <h3>En progreso</h3>
        <Plus @click="mostrarInput(2)" class="iconoClickable" />
      </div>
      <hr style="margin-block: 5px;">
      <draggable class="grupoTareas" :list="tablaProgreso" group="tareas" itemKey="nombre" :empty-insert-threshhold="500"
        @change="actualizarLocalStorage">
        <template #item="{ element, index }">
          <div class="itemTarea">
            <div class="descripcionTarea" @click="abrirModalEditar(element, index)">
              <div class="tareaTitulo">
                {{ element.nombre }}
              </div>
              <div class="tareaDescripcion" v-if="element.descripcion">
                {{ element.descripcion }}
              </div>
              <div class="tareaDescripcion" v-if="element.fecha">
                {{ formatearFecha(element.fecha) }}
              </div>
            </div>
            <div>
              <Close @click="eliminarTarea(index, tablaProgreso, 'tablaProgreso')" class="iconoClickable" />
            </div>
          </div>
        </template>
      </draggable>

      <div v-if="mostrarInputProgreso" class="itemTarea itemTareaShadow" style="display:flex; margin-top:5px;">
        <input v-model="nombreTareaProgreso" @keyup.enter="agregarTarea(nombreTareaProgreso, tablaProgreso)"
          class="inputTarea" />
        <Check @click="agregarTarea(nombreTareaProgreso, tablaProgreso, 'tablaProgreso')" class="iconoClickable" />
      </div>
    </div>

    <div class="tabla">
      <div class="tablaHeader">
        <h3>Hecho</h3>
        <Plus @click="mostrarInput(3)" class="iconoClickable" />
      </div>
      <hr style="margin-block: 5px;">
      <draggable class="grupoTareas" :list="tablaHecho" group="tareas" itemKey="nombre" :empty-insert-threshhold="500"
        @change="actualizarLocalStorage">
        <template #item="{ element, index }">
          <div class="itemTarea">
            <div class="descripcionTarea" @click="abrirModalEditar(element, index)">
              <div class="tareaTitulo">
                {{ element.nombre }}
              </div>
              <div class="tareaDescripcion" v-if="element.descripcion">
                {{ element.descripcion }}
              </div>
              <div class="tareaDescripcion" v-if="element.fecha">
                {{ formatearFecha(element.fecha) }}
              </div>
            </div>
            <div>
              <Close @click="eliminarTarea(index, tablaHecho, 'tablaHecho')" class="iconoClickable" />
            </div>
          </div>
        </template>
      </draggable>

      <div v-if="mostrarInputHecho" class="itemTarea itemTareaShadow" style="display:flex; margin-top:5px;">
        <input v-model="nombreTareaHecho" @keyup.enter="agregarTarea(nombreTareaHecho, tablaHecho, 'tablaHecho')"
          class="inputTarea" />
        <Check @click="agregarTarea(nombreTareaHecho, tablaHecho, 'tablaHecho')" class="iconoClickable" />
      </div>
    </div>
    <EditarModal v-if="mostrarModal" :tareaSeleccionada="tareaSeleccionada" @actualizar-tarea="actualizarTarea"
      @cerrar-modal="cerrarModal" />
  </div>
</template>

<script>
import draggable from "vuedraggable";
import Plus from 'vue-material-design-icons/Plus.vue';
import Check from 'vue-material-design-icons/Check.vue';
import Close from 'vue-material-design-icons/Close.vue';
import EditarModal from '../components/EditarModal.vue'

export default {
  components: {
    draggable,
    Plus,
    Check,
    Close,
    EditarModal
  },
  data() {
    return {
      tablaPendiente: [
      ],
      tablaProgreso: [
      ],
      tablaHecho: [
      ],

      mostrarInputPendiente: false,
      mostrarInputProgreso: false,
      mostrarInputHecho: false,
      nombreTareaPendiente: "",
      nombreTareaProgreso: "",
      nombreTareaHecho: "",

      //LOCALSTORAGE, tambien en la funcion agregar
      tablaPendiente: JSON.parse(localStorage.getItem('tablaPendiente')) || [],
      tablaProgreso: JSON.parse(localStorage.getItem('tablaProgreso')) || [],
      tablaHecho: JSON.parse(localStorage.getItem('tablaHecho')) || [],


      mostrarModal: false,
      tareaSeleccionada: null,
    };
  },
  methods: {
    //mostrar el input en el cual se puede escribir el nombre de la nueva tarea
    mostrarInput(tabla) {
      if (tabla === 1) {
        this.mostrarInputPendiente = true;
      } else if (tabla === 2) {
        this.mostrarInputProgreso = true;
      } else if (tabla === 3) {
        this.mostrarInputHecho = true;
      }
    },

    //agregar nueva tarea y esconder el escribir
    agregarTarea(nombre, tabla, keyLocalStorage) {
      if (nombre.trim() !== "") {
        this.agregar(nombre, tabla, keyLocalStorage);
        //resetear los inputs
        this.nombreTareaPendiente = "";
        this.mostrarInputPendiente = false;
        this.nombreTareaProgreso = "";
        this.mostrarInputProgreso = false;
        this.nombreTareaHecho = "";
        this.mostrarInputHecho = false;
      }
    },

    agregar: function (nombre, tabla, keyLocalStorage) {
      tabla.push({ nombre: nombre });

      // LOCALSTORAGE
      localStorage.setItem(keyLocalStorage, JSON.stringify(tabla));
    },

    eliminarTarea: function (index, tabla, keyLocalStorage) {
      if (index !== -1) {
        tabla.splice(index, 1);
        // LOCALSTORAGE
        localStorage.setItem(keyLocalStorage, JSON.stringify(tabla));
      }
    },


    actualizarLocalStorage: function () {
      localStorage.setItem('tablaPendiente', JSON.stringify(this.tablaPendiente));
      localStorage.setItem('tablaProgreso', JSON.stringify(this.tablaProgreso));
      localStorage.setItem('tablaHecho', JSON.stringify(this.tablaHecho));
      console.log("actualizado")
    },

    abrirModalEditar(tarea, index) {
      this.tareaSeleccionada = { ...tarea, index };
      this.mostrarModal = true;
    },
    cerrarModal() {
      this.tareaSeleccionada = null;
      this.mostrarModal = false;
    },
    actualizarTarea(tareaActualizada) {
      const indice = tareaActualizada.index;
      if (indice !== undefined && indice >= 0) {
        //encontrar la tarea en cada una de las tablas
        [this.tablaPendiente, this.tablaProgreso, this.tablaHecho].forEach((tabla, tabIndex) => {
          const tareaAReemplazar = tabla[indice];
          //debe coincidir el nombre, si se tuviera id, utilizaria el id, pero se complicaria mas usando localstorage
          if (tareaAReemplazar !== undefined && tareaAReemplazar.nombre === this.tareaSeleccionada.nombre) {

            //reemplazar tarea
            tabla.splice(indice, 1, tareaActualizada);
            this.mostrarModal = false;
            //localStorage
            const localStorageKey = tabIndex === 0 ? 'tablaPendiente' : tabIndex === 1 ? 'tablaProgreso' : 'tablaHecho';
            localStorage.setItem(localStorageKey, JSON.stringify(tabla));
          }
        });
      }
    },

    formatearFecha(fecha){
      console.log(fecha)
      let newFecha = fecha;
      if (typeof(fecha)==='Date') {
        newFecha = newFecha.toLocaleDateString("en-US");
      }
      newFecha = newFecha.toString()
      return newFecha.split('T')[0].split('-').reverse().join('/');
    }

  },

  //Localstorage
  beforeMount() {
    if (localStorage.getItem('tablaPendiente')) {
      this.tablaPendiente = JSON.parse(localStorage.getItem('tablaPendiente'));
    }
    if (localStorage.getItem('tablaProgreso')) {
      this.tablaProgreso = JSON.parse(localStorage.getItem('tablaProgreso'));
    }
    if (localStorage.getItem('tablaHecho')) {
      this.tablaHecho = JSON.parse(localStorage.getItem('tablaHecho'));
    }
    console.log("datos cargados");
  }


};
</script>