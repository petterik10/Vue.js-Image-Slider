<template>
  <div>
    <li>
      <div class="settings-interval">
        <span class="settings-interval-label"> {{ name }} </span>
        <input
          v-if="!selectElement"
          v-model.number="inputValue"
          type="number"
          class="settings-interval-input"
          @focus="$emit('setPlayingToFalse')"
        />
        <select v-else class="app-interval-input" v-model="thumbnailSelection">
          <option :key="option" v-for="option in options">
            {{ option }}
          </option>
        </select>
      </div>
    </li>
  </div>
</template>

<script>
export default {
  name: "Options",
  props: ["name", "modelValue", "selectElement"],

  data: function() {
    return {
      options: ["Bottom", "Right", "Left", "Top"],
    };
  },

  computed: {
    inputValue: {
      get() {
        return this.modelValue;
      },
      set(value) {
        this.$emit("update:modelValue", value);
      },
    },
    thumbnailSelection: {
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
.settings-buttons {
  padding: 0;
  margin: 0;
  list-style: none;

  li {
    display: block;
  }

  .settings-interval {
    display: table;
  }

  .settings-interval-label {
    display: table-cell;
    vertical-align: middle;
    padding: 6px 12px;
    font-size: 14px;
    font-weight: 400;
    line-height: 1;
    color: #555;
    background-color: #eee;
    border: 3px solid #ccc;
    border-right: none;
    border-radius: 4px;
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;

    @media (min-width: 400px) {
      text-align: center;
    }
  }

  .settings-interval-input {
    width: 65px;
    -webkit-appearance: none;
    display: table-cell;
    margin: 0;
    padding: 9px;
    border-radius: 5px;
    font-size: 14px;
    border: 3px solid #ccc;
    background: #fff;
    width: 100%;
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
  }
}
.app-interval-input {
  -webkit-appearance: none;
  display: table-cell;
  margin: 0;
  padding: 9px;
  border-radius: 5px;
  font-size: 14px;
  border: 3px solid #ccc;
  background: #fff;
  width: 100%;
  border-top-left-radius: 0;
  border-bottom-left-radius: 0;
}
</style>
