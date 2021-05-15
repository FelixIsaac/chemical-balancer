<template>
  <div id="app">
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
      <Buttons @generateEquation="value = $event" />
      <el-alert
        type="success"
        class="display"
        :closable="false"
        v-show="solution"
        show-icon
        center
      >
        <div v-html="solution" style="font-size: 30px"/>
      </el-alert>
      <el-alert
        type="error"
        class="display"
        :closable="false"
        v-show="error"
        show-icon
        center
      >
        <span style="font-size: 30px">{{ error }}</span>
      </el-alert>
    </el-main>
  </div>
</template>

<script>
// import Balancer from './components/Balancer.vue';
import { balance as balanceEquation } from './library/chem-balancer';
import Buttons from './components/Buttons.vue';

export default {
  name: 'App',
  components: {
    Buttons,
  },
  data: () => ({
    isBalancing: false,
    solution: null,
    error: null,
    value: '',
  }),
  methods: {
    balance() {
      this.isBalancing = true;
      this.solution = null;
      this.error = null;

      try {
        const { eqn, coefs } = balanceEquation(this.value);

        setTimeout(() => {
          this.isBalancing = false;
          this.solution = eqn.toHtml(coefs).outerHTML;
        }, 300);
      } catch (err) {
        this.isBalancing = false;
        this.error = err.toString();
      }
    },
    submit(e) {
      e.preventDefault();
      this.balance();
    },
  },
  watch: {
    initialVisibility: console.log,
    value() {
      this.balance();
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<style scoped>
h1 {
  font-size: 60px;
  mar-bottom: 10px;
}

.equation {
  width: 50vw;
}

.display {
  margin: 30px 0;
  padding: 30px;
}
</style>
