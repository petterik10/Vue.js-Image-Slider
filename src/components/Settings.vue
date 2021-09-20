<template>
  <div class="settings-container">
    <div class="settings-container--inner">
      <h2>Settings</h2>
      <ul class="settings-buttons">
        <Options
          :change="$emit('intervalTime', intervalTime)"
          v-model="intervalTime"
          name="Play Interval"
          @setPlayingToFalse="$emit('setPlayingToFalse')"
          :selectElement="false"
        />
        <Options
          :change="$emit('changeSlideInterval', slideIntervalTime)"
          v-model="slideIntervalTime"
          name="Slide Interval"
          @setPlayingToFalse="$emit('setPlayingToFalse')"
          :selectElement="false"
        />

        <Options
          name="Thumbnail Position"
          v-model="thumbnailPosition"
          :change="$emit('changeThumbnailPosition', thumbnailPosition)"
          :selectElement="true"
          :showThumbnailSelection="showThumbnailSelection"
        />
      </ul>
      <ul class="settings-checkboxes">
        <li>
          <Checkbox
            label="show Bullets"
            value="bullets"
            v-model="checkedValues"
            :change="$emit('update-checked', checkedValues)"
          />
        </li>
        <li>
          <Checkbox
            label="show Thumnbnails"
            value="thumbnails"
            v-model="checkedValues"
            :change="$emit('update-checked', checkedValues)"
          />
        </li>
        <li>
          <Checkbox
            label="show Index"
            value="index"
            v-model="checkedValues"
            :change="$emit('update-checked', checkedValues)"
          />
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import Checkbox from "./Checkbox.vue";
import Options from "./Options.vue";

export default {
  name: "Settings",
  components: {
    Options,
    Checkbox,
  },
  props: {
    slideInterval: { type: Number },
    checkboxes: { type: Array },
  },
  data: function() {
    return {
      checkedCheckboxes: [],
      intervalTime: 2000,
      slideIntervalTime: 450,
      thumbnailPosition: "Bottom",
      checkedValues: ["bullets", "thumbnails", "index"],
    };
  },
  computed: {
    showThumbnailSelection() {
      return !this.checkedValues.includes("thumbnails");
    },
  },
  emits: [
    "intervalTime",
    "setPlayingToFalse",
    "changeSlideInterval",
    "changeThumbnailPosition",
    "changeShowBullets",
    "update-checked",
  ],
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.settings-container {
  margin: 40px auto;
  width: 65%;

  @media (max-width: 1024px) {
    width: 100%;
  }
}

.settings-buttons {
  padding: 0;
  margin: 0;
  display: flex;
  list-style: none;
  gap: 10px;
  flex-wrap: wrap;
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

.settings-checkboxes {
  margin-top: 30px;
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  li {
    display: block;
  }
}
</style>
