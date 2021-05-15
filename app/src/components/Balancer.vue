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
    <div>
      <el-button
        type="primary"
        icon="el-icon-refresh"
        @click="random"
      >Solve random equation</el-button>
      <el-button
        type="primary"
        icon="el-icon-view"
        @click="syntaxGuide = !syntaxGuide"
      >
      {{ syntaxGuide ? "Hide" : "Show" }} syntax guide
    </el-button>
  </div>
    <el-dialog title="Syntax Guide" :visible.sync="syntaxGuide">
      <el-table :data="rows">
        <el-table-column property="type" label="Types"></el-table-column>
        <el-table-column property="example" label="Examples"></el-table-column>
        <el-table-column property="output" label="Output"></el-table-column>
      </el-table>
    </el-dialog>
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
    error: null,
    syntaxGuide: false,
    value: '',
    rows: [
      {
        type: 'Subscripts',
        example: 'N = N2',
        output: 'N → N2',
      },
      {
        type: 'Compounds',
        example: 'H2 + O2 = H2O',
        output: 'H2 + O2 → H2O',
      },
      {
        type: 'Groups',
        example: 'Mg(OH)2 = MgO + H2O',
        output: 'Mg(OH)2 → MgO + H2O',
      },
      {
        type: 'Ions',
        example: 'H^+ + CO3^2- = H2O + CO2',
        output: 'H+ + CO32− → H2O + CO2',
      },
      {
        type: 'Electrons',
        example: 'Fe^3+ + e = Fe',
        output: 'Fe3+ + e− → Fe',
      },
      {
        type: 'No space',
        example: 'A3^-+B2^2+=A5B+e',
        output: 'A3− + B22+ → A5B + e−',
      },
      {
        type: 'More space',
        example: 'C 3 H 5 ( O H ) 3 + O 2 = H 2 O + C O 2',
        output: 'C3H5(OH)3 + O2 → H2O + CO2',
      },
      {
        type: 'Optional 1',
        example: 'H1^1+ + e = H1^1-',
        output: 'H+ + e− → H−',
      },
      {
        type: 'Flexible names',
        example: 'Foo^5+ + Bar^3- = FooBar2 + FooBar^-',
        output: 'Foo5+ + Bar3− → FooBar2 + FooBar−',
      },
    ],
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

.display {
  margin: 30px 0;
  padding: 30px;
}
</style>
