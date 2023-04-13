<template>
  <h2 :class="hover ? 'blue-text' : 'green-text'">{{ msg }}</h2>
  <img
    alt="Image of monkey"
    src="https://images.unsplash.com/flagged/photo-1566127992631-137a642a90f4?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1170&q=80"
    class="contain"
    @mouseover="hover = true"
    v-if="!hover"
  />
  <img
    alt="Image of Trump"
    src="https://www.rollingstone.com/wp-content/uploads/2023/03/trumps-revenge-on-bragg.jpg?w=1581&h=1054&crop=1"
    class="contain"
    @mouseleave="hover = false"
    v-if="hover"
  />
</template>

<script>
export default {
  name: 'TestComponent',
  props: {
    msg: String,
  },
  data() {
    return {
      hover: false,
    };
  },
};
</script>

<style scoped>
.green-text {
  margin: 40m 0 0;
  color: green;
}

.blue-text {
  margin: 40m 0 0;
  color: lightblue;
}

.contain {
  width: 50%;
  height: auto;
}
</style>
