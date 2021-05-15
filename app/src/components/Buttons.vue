<template>
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
    <SyntaxGuide :initialVisibility="syntaxGuide" @visibility="syntaxGuide = $event" />
  </div>
</template>

<script>
import SyntaxGuide from './SyntaxGuide.vue';
import { random as generateEquation } from '../library/chem-balancer';

export default {
  name: 'Buttons',
  components: { SyntaxGuide },
  props: {
    randomEquation: Function,
  },
  data() {
    return {
      syntaxGuide: false,
    };
  },
  methods: {
    random() {
      this.$emit('generateEquation', generateEquation());
    },
  },
};
</script>
