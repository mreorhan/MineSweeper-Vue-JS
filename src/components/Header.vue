<template>
  <div class="d-flex justify-content-between mx-4 my-4">
    <img src="../assets/LOGO.png" width="240" />
    <div class="score align-self-center">Score: {{totalPoints}}</div>
  </div>
</template>

<script>
export default {
  name: "Header",
  props: ["totalPoints"]
};
</script>