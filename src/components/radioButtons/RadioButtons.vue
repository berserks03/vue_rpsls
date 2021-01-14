<template>
  <div v-for="(choice, index) in choices" :key="index" >
    <input
      type="radio"
      :id="choice"
      :value="choice"
      v-model="pickedRadio"
      @change="sendRadioValue"
    />
    <label class="radioLabel" :for="choice">{{choice}}</label>
    <br />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  data() {
    return {
      choices: ['Random', 'Counter most used', 'Both'],
      pickedRadio: 'Both',
    };
  },
  methods: {
    sendRadioValue() {
      this.$emit('radioValueUpdated', this.pickedRadio);
    },
  },
});
</script>

<style scoped lang="scss">

</style>
