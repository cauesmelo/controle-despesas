<template>
  <header>
    <div class="headerContent">
      <h1>
        <div class="mainIcon"></div>
        Controle de despesas
      </h1>

      <div class="totalLine">
        <div class="totalCard">
          <div class="totalCardHeader">
            <h3>Entradas</h3>
            <div class="iconTotal"></div>
          </div>
          <p>R$ {{ formatPrice(income) }}</p>
        </div>

        <div class="totalCard">
          <div class="totalCardHeader">
            <h3>Saídas</h3>
            <div class="iconTotal"></div>
          </div>
          <p>R$ {{ formatPrice(outcome) }}</p>
        </div>

        <div class="totalCard">
          <div class="totalCardHeader">
            <h3>Total</h3>
            <div class="iconTotal"></div>
          </div>
          <p>R$ {{ formatPrice(total) }}</p>
        </div>
      </div>
    </div>
  </header>

  <main>
    <button class="btnAdicionar" @click="showModal">
      <div class="iconAdd"></div>
      Adicionar novo
    </button>
    <table>
      <tr>
        <th>Descrição</th>
        <th>Valor</th>
      </tr>
      <tr v-for="(transaction, index) in transactions" :key="index">
        <td>{{ transaction.desc }}</td>
        <td>R$ {{ formatPrice(transaction.val) }}</td>
      </tr>
    </table>
  </main>
  <ModalAdd v-show="isModalVisible" @close="closeModal" @add="add" />
</template>

<script>
import ModalAdd from "../components/ModalAdd/ModalAdd.vue";

export default {
  name: "App",
  data() {
    return {
      id: 1,
      isModalVisible: false,
      income: 0,
      outcome: 0,
      total: 0,
      transactions: [],
    };
  },
  components: {
    ModalAdd,
  },
  methods: {
    calculate() {
      this.income = 0;
      this.outcome = 0;
      this.total = 0;
      this.transactions.forEach((t) => {
        if (t.val > 0) this.income += t.val;
        if (t.val < 0) this.outcome += t.val;
        this.total += t.val;
      });
    },
    showModal: function () {
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    },
    add: function (e) {
      this.transactions.push({
        id: this.id++,
        desc: e.desc,
        val: parseInt(e.val),
      });
      this.calculate();
    },
    formatPrice(value) {
      let val = (value / 1).toFixed(2).replace(".", ",");
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
    },
  },
};
</script>

<style src="../styles/app.module.scss" lang="scss" /> 