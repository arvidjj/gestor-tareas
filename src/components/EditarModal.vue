<script>
export default {
  props: ['tareaSeleccionada'],
  data() {
    return {
      tareaEditada: { ...this.tareaSeleccionada },
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
      <div class="modalForm"><input v-model="tareaEditada.nombre" class="inputTarea" /></div>
      <div class="modalBotones">
        <button @click="guardarCambios">Guardar</button>
        <button @click="cerrarModal">Cancelar</button>
      </div>
    </div>
  </div>
</template>
