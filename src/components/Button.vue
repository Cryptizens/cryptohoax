<template>
  <div class="row">
    <div class="col-sm-12 text-center">
      <button @click="callRandomizer()">GIMME MORE</button>
    </div>
  </div>
</template>

<script>
import { EventBus } from '../event-bus.js';
export default {
  methods: {
    callRandomizer() {
      EventBus.$emit('randomizer-called', 1);
    }
  }
}
</script>

<style lang="scss">
button {
  display: inline-block;
  border-radius: 3px;
  padding: 0.5rem 0;
  margin: 1rem 1rem;
  margin-top: 20px;
  width: 11rem;
  background: transparent;
  color: white;
  border: 2px solid white;
  font-weight: bold;
  cursor: pointer;

  &:hover {
    background: white;
    color: palevioletred;
  }
}

button:focus {outline:0;}
</style>
