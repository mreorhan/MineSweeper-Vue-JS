<template>
  <table class="mine-table">
    <template v-for="(row,index) in tableItems">
      <tr :key="index">
        <template v-for="(item,index2) in row">
          <td :key="index2">
            <template v-if="!item.selected">
              <img
                width="90"
                height="90"
                @click="$emit('onPress',item)"
                src="../assets/Rectangle.png"
              />
            </template>
            <template v-else-if="item.type =='tick'">
              <img width="90" height="90" src="../assets/Clear.png" />
            </template>
            <template v-else>
              <img width="90" height="90" src="../assets/Mine.png" />
            </template>
          </td>
        </template>
      </tr>
    </template>
  </table>
</template>

<script>
export default {
  name: "MineTable",
  props: ["tableItems"]
};
</script>


<style scoped>
.mine-table tr td {
  padding: 6px;
}
</style>
