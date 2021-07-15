<template>
  <transition name="fade">
    <div class="container" @keyup.esc="$emit('close')" tabindex="0">
      <div class="modalBox">
        <h2>
          <div class="iconAdd"></div>
          Criar registro
        </h2>
        <div class="modalContent">
          Insira descrição
          <input
            type="text"
            v-model="newDescription"
            placeholder="Digite aqui a descrição da transação"
          />
          Insira valor
          <input
            type="text"
            v-model="newValue"
            placeholder="R$ 9,00"
            @keyup.enter="add"
          />
          <div class="containerButtons">
            <button @click="close">Cancelar</button>
            <button @click="add">Inserir</button>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>


<script>
export default {
  name: "Modal",
  data() {
    return {
      newDescription: "",
      newValue: "",
    }
  },
  methods: {
    close() {
      this.$emit("close");
      this.newDescription = "";
      this.newValue = "";
    },
    add() {
      this.$emit("add", { desc: this.newDescription, val: this.newValue });
      this.close();
    },
  },
};
</script>

<style src="./ModalAdd.module.scss" lang="scss" scoped/>