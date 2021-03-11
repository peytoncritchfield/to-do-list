<template>

  <div class="hello">
Task: 
{{msg}}


  </div>

</template>

<script>

export default {
  name: 'HelloWorld',
  props: ["msg"]
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>
.hello {
    background-color: #eee;
    font-size: 22px;
    color: green;
    margin: 1em auto;
}

</style>