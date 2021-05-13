<template>
  <el-main>
    <h1>Chemical Balancer</h1>
    <el-form inline status-icon @submit.native="submit">
      <el-form-item>
        <el-input
          placeholder="Chemical equation"
          class="equation"
          v-model="value"
          clearable
        />
      </el-form-item>
      <el-form-item>
        <el-button
          type="primary"
          icon="el-icon-magic-stick"
          @click="balance"
          :loading="isBalancing"
        >
          {{ isBalancing ? "Balancing..." : "Balance"}}
        </el-button>
      </el-form-item>
    </el-form>
    <el-button-group>
      <el-button
        type="primary"
        icon="el-icon-refresh"
        @click="random"
      >Solve random equation</el-button>
    </el-button-group>
    <el-alert
      type="success"
      class="solution"
      show-icon
      center
      :closable="false"
      v-show="solution"
    >
      <div v-html="solution" style="font-size: 30px"/>
    </el-alert>
  </el-main>
</template>

<script>
import {
  balance as balanceEquation,
  random as randomEquation,
} from '../library/chem-balancer';

export default {
  name: 'Balancer',
  components: {},
  data: () => ({
    isBalancing: false,
    solution: null,
    value: '',
  }),
  methods: {
    balance() {
      this.isBalancing = true;
      this.solution = null;

      const { eqn, coefs } = balanceEquation(this.value);

      setTimeout(() => {
        this.isBalancing = false;
        this.solution = eqn.toHtml(coefs).outerHTML;
        console.log(this.solution);
      }, 300);
    },
    submit(e) {
      e.preventDefault();
      this.balance();
    },
    random() {
      this.value = randomEquation();
      this.balance();
    },
  },
};
</script>

<style scoped>
h1 {
  font-size: 60px;
  mar-bottom: 10px;
}

.equation {
  width: 50vw;
}

.solution {
  margin: 30px 0;
  padding: 30px;
}
</style>
