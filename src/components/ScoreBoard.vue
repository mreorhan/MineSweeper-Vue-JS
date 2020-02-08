<template>
  <div class="card score-board mr-4">
    <div class="card-body">
      <h5 class="card-title">Top 10 Scores</h5>
      <div
        class="modal-body d-flex justify-content-between"
        v-for="(item,index) in highScores"
        :key="index"
      >
        <div>{{(index+1)+". score: "+item.name}}</div>
        <div>{{item.score}}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ScoreBoard",
  props: ["highScores"]
};
</script>


<style>
.score {
  padding: 5px 15px;
  background-color: #f7f9fa;
  color: #91949f;
  border-radius: 5px;
  border: 1px solid #91949f;
}
.score-board {
  width: 18rem;
  margin: 8px;
}
</style>