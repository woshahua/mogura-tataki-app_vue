<template>
  <div class="counter">
    <h2>{{title}}:</h2>
    <h1>{{value}}</h1>
  </div>
</template>
<script>
export default {
  name: "counter",
  props:{
      title: String,
      value: Number,
      itemId: Number,
  },
};
</script>