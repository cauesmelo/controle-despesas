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
          <div
            v-bind:class="[
              errorDesc ? 'containerInput errorUser' : 'containerInput',
            ]"
          >
            <input
              @focus="errorDesc = false"
              type="text"
              v-model="newDescription"
              placeholder="Digite aqui a descrição da transação"
            />
            <div v-if="errorDesc" class="iconError"></div>
          </div>

          Insira valor
          <div
            v-bind:class="[
              errorAmount ? 'containerInput errorUser' : 'containerInput',
            ]"
          >
            <money3
              @focus="errorAmount = false"
              v-model="amount"
              v-bind="config"
              @keyup.enter="add"
            ></money3>
            <div v-if="errorAmount" class="iconError"></div>
          </div>
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
import { Money3Component } from "v-money3";
import { createToast } from "mosha-vue-toastify";
import "mosha-vue-toastify/dist/style.css";

function sleep(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

export default {
  components: { money3: Money3Component },
  name: "Modal",
  data() {
    return {
      newDescription: "",
      errorAmount: false,
      errorDesc: false,
      amount: 0,
      config: {
        decimal: ",",
        thousands: ".",
        prefix: "R$ ",
        suffix: "",
        precision: 2,
        masked: false,
        disableNegative: false,
        disabled: false,
        min: Number.MIN_SAFE_INTEGER,
        max: Number.MAX_SAFE_INTEGER,
        allowBlank: false,
        minimumNumberOfCharacters: 0,
      },
    };
  },
  methods: {
    close() {
      this.$emit("close");
      this.newDescription = "";
      this.amount = 0;
      this.errorAmount = false;
      this.errorDesc = false;
    },
    async add() {
      if (this.newDescription === "") {
        createToast("Insira uma descrição válida", { type: "danger" });
        this.errorDesc = true;
      } else {
        this.errorDesc = false;
      }
      if (this.amount === 0) {
        this.errorAmount = true;
        await sleep(5);
        createToast("Insira um valor válido", { type: "danger" });
      } else {
        this.errorAmount = false;
      }

      if (this.amount !== 0 && this.newDescription !== "") {
        this.$emit("add", { desc: this.newDescription, val: this.amount });
        this.close();
      }
    },
  },
};
</script>

<style src="./ModalAdd.module.scss" lang="scss" scoped/>