<script>
import Header from './components/Header.vue';
import Pokemon from './components/Pokemon.vue';

export default {
  components: {
    Pokemon,
    Header,
  },
  data() {
    return {
      totalActivos: 0
    }
  },
  mounted() {
    const myModalElement = this.$refs.myModal;
    const myInputElement = this.$refs.myInput;

    if (myModalElement) {
      const modalInstance = new bootstrap.Modal(myModalElement);
      const myInput = document.getElementById('myInput')
      
      myModal.addEventListener('shown.bs.modal', () => {myInput.focus()});
    }
  },
  methods: {
    conteoActivos(conteo) {
      this.totalActivos = conteo
    },
    mostrarModal() {
      // Mostrar el modal cuando el evento `nombre-incorrecto` es emitido
      const modal = new bootstrap.Modal(this.$refs.myModal);
      modal.show();
    }

  }
}
</script>

<template>
  <Header :activos="totalActivos"/>
  <Pokemon @nombreIncorrecto="mostrarModal" @conteoActivos="conteoActivos"/>

  <div class="modal" tabindex="-1" id="myModal" ref="myModal">
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Nombre incorrecto</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <p>El nombre ingresado no coincide con el Pokémon. <img class="pikachu" src="./assets/img/pikachu.png" alt=""></p>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
          </div>
        </div>
      </div>
    </div>
</template>

<style scoped>
.modal-content{
  background-color: rgb(206, 120, 7);
  color: white;
}
.modal-body{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}
.pikachu {
  width: 200px;
  height: auto;
  margin: 10px;
}
</style>
