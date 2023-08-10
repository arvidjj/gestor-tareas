<script>
import VueDatePicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css'

export default {
  props: ['tareaSeleccionada'],
  components: { VueDatePicker },
  data() {
    return {
      maxTitulo: 12,
      maxDescripcion: 30,
      tareaEditada: { ...this.tareaSeleccionada },
      fecha: null,
    };
  },
  methods: {
    guardarCambios() {
      this.$emit('actualizar-tarea', this.tareaEditada);
    },
    cerrarModal() {
      this.$emit('cerrar-modal');
    },
  },
};
</script>

<template>
  <div class="modal">
    <div class="modalContent">
      <h2>Editar Tarea</h2>
      <div class="modalForm">
        <div>
          <input v-model="tareaEditada.nombre" :maxlength="maxTitulo" class="inputTarea" />
        </div>
        <div>
          <p>Descripcion: </p>
          <input v-model="tareaEditada.descripcion" :maxlength="maxDescripcion" class="inputTareaDescripcion" />
        </div>
        <div>
          <p>Asignado a: </p>
          <input v-model="tareaEditada.asignadoA" :maxlength="maxDescripcion" class="inputTareaDescripcion" />
        </div>
        <div>
          <VueDatePicker v-model="tareaEditada.fecha" inline auto-apply :enable-time-picker="false" style="display: flex; align-items: center; justify-content: center;"/>
        </div>
      </div>
      <div class="modalBotones">
        <button @click="guardarCambios">Guardar</button>
        <button @click="cerrarModal">Cancelar</button>
      </div>
    </div>
  </div>
</template>
