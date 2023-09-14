<template>
    <p>Registration form here</p>
    <button @click="register">Register Me!</button>
</template>
  
  <script>
  export default {
    props: ['event'],
    methods: {
      register() {
        // When register, go back to the events details
        this.$router.push({ name: 'EventDetails' })
      }
    }
  }
  </script>
  