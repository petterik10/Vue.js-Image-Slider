<template>
  <input v-model="inputValue" :value="value" type="checkbox" />
  <label :for="label"> {{ label }} </label>
</template>

<script>
export default {
  name: "Checkbox",
  props: ["label", "value", "modelValue"],

  computed: {
    inputValue: {
      get() {
        return this.modelValue;
      },
      set(value) {
        this.$emit("update:modelValue", value);
      },
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
label {
  padding: 0 5px;
}
</style>
