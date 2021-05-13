<template>
  <main>
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
  </main>
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
    value: '',
  }),
  methods: {
    balance() {
      this.isBalancing = true;

      const { eqn, coefs } = balanceEquation(this.value);
      console.log(eqn.toHtml(coefs));

      this.isBalancing = false;
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
  width: 70vw;
}
</style>
