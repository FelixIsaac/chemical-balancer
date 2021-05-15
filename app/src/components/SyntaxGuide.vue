<template>
  <el-dialog title="Syntax Guide" :visible.sync="visible">
    <el-table :data="rows">
      <el-table-column property="type" label="Types"></el-table-column>
      <el-table-column property="example" label="Examples"></el-table-column>
      <el-table-column property="output" label="Output"></el-table-column>
    </el-table>
  </el-dialog>
</template>

<script>
export default {
  name: 'SyntaxGuide',
  props: {
    initialVisibility: Boolean,
  },
  data() {
    return {
      visible: this.initialVisibility,
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
    };
  },
  watch: {
    visible() {
      this.$emit('visibility', this.visible);
    },
    initialVisibility() {
      this.visible = this.initialVisibility;
    },
  },
};
</script>
