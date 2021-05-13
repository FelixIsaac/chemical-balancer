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
        <el-button type="primary" @click="balance">Balance</el-button>
      </el-form-item>
    </el-form>
  </main>
</template>

<script>
export default {
  name: 'Balancer',
  components: {},
  data: () => ({
    isLoading: false,
    value: '',
  }),
  methods: {
    balance() {
      console.log(this.value);
    },
    submit(e) {
      e.preventDefault();
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
