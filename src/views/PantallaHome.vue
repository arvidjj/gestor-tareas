<template>
  <div class="containerTareas">
    <div class="tabla">
      <div class="tablaHeader">
        <h3>Pendiente</h3>
        <Plus @click="mostrarInput(1)" class="iconoClickable" />

      </div>
      <hr style="margin-block: 5px;">
      <draggable class="grupoTareas" :list="tablaPendiente" group="tareas" itemKey="nombre" :empty-insert-threshhold="500" @change="actualizarLocalStorage">
        <template #item="{ element, index }">
          <div class="itemTarea">{{ element.nombre }}</div>
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
      <draggable class="grupoTareas" :list="tablaProgreso" group="tareas" itemKey="nombre" :empty-insert-threshhold="500" @change="actualizarLocalStorage">
        <template #item="{ element, index }">
          <div class="itemTarea">{{ element.nombre }}</div>
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
      <draggable class="grupoTareas" :list="tablaHecho" group="tareas" itemKey="nombre" :empty-insert-threshhold="500" @change="actualizarLocalStorage">
        <template #item="{ element, index }">
          <div class="itemTarea">{{ element.nombre }}</div>
        </template>
      </draggable>

      <div v-if="mostrarInputHecho" class="itemTarea itemTareaShadow" style="display:flex; margin-top:5px;">
        <input v-model="nombreTareaHecho" @keyup.enter="agregarTarea(nombreTareaHecho, tablaHecho, 'tablaHecho')" class="inputTarea" />
        <Check @click="agregarTarea(nombreTareaHecho, tablaHecho, keyLocalStorage)" class="iconoClickable" />
      </div>
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
import Plus from 'vue-material-design-icons/Plus.vue';
import Check from 'vue-material-design-icons/Check.vue';

export default {
  components: {
    draggable,
    Plus,
    Check
  },
  data() {
    return {
      tablaPendiente: [
        {
          nombre: "Tarea 1", id: 1
        }, {
          nombre: "Tarea 2", id: 2
        },
      ],
      tablaProgreso: [
        { nombre: "Tarea 3", id: 3 },
      ],
      tablaHecho: [
        { nombre: "Tarea 4", id: 4 },
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

      //LOCALSTORAGE
      localStorage.setItem(keyLocalStorage, JSON.stringify(tabla));
    },

    actualizarLocalStorage: function () {
      localStorage.setItem('tablaPendiente', JSON.stringify(this.tablaPendiente));
      localStorage.setItem('tablaProgreso', JSON.stringify(this.tablaProgreso));
      localStorage.setItem('tablaHecho', JSON.stringify(this.tablaHecho));
      console.log("actualizado")
    }
  },

  //Localstorage
  beforeMount() {
    //cargar datos, si no existe, guardar
    if (!localStorage.getItem('tablaPendiente')) {
      localStorage.setItem('tablaPendiente', JSON.stringify(this.tablaPendiente));
    }
    if (!localStorage.getItem('tablaProgreso')) {
      localStorage.setItem('tablaProgreso', JSON.stringify(this.tablaProgreso));
    }
    if (!localStorage.getItem('tablaHecho')) {
      localStorage.setItem('tablaHecho', JSON.stringify(this.tablaHecho));
    }

    console.log("datos cargados")
  }
};
</script>