<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import { range, sampleSize, shuffle } from 'lodash';
import { ref } from 'vue';
const bg = localStorage.getItem('bingo')
let bingo: Array<Array<String>>;
if (bg == undefined) {
  const first = shuffle(sampleSize(range(1, 15).map(item => String(item)), 5))
  const second = shuffle(sampleSize(range(16, 30).map(item => String(item)), 5))
  const third = shuffle(sampleSize(range(31, 45).map(item => String(item)), 4))
  third.splice(2, 0, ('BINGO'))
  const forth = shuffle(sampleSize(range(46, 60).map(item => String(item)), 5))
  const fifth = shuffle(sampleSize(range(61, 75).map(item => String(item)), 5))
  bingo = [first, second, third, forth, fifth]
  localStorage.setItem('bingo', JSON.stringify(bingo))
} else {
  bingo = JSON.parse(bg)
}
const colors = ref<Array<Array<boolean>>>([])
for (let i in range(5)) {
  colors.value.push([false, false, false, false, false])
}
</script>

<template>
  <div style="display: inline-block; margin: auto; border-left: solid; border-top: solid;">
    <div class="cell-parent" v-for="(row, findex) in bingo">
      <div
        :class="{ red: colors[findex][sindex] == true }"
        @click="colors[findex][sindex] = !colors[findex][sindex]"
        class="cell"
        v-for="(item, sindex) in row"
      >{{ item }}</div>
    </div>
  </div>
</template>
<style>
.cell-parent {
  display: flex;
  align-items: center;
  justify-content: center;
}
.cell {
  width: 100px;
  height: 100px;
  border-right: solid;
  border-bottom: solid;
  display: grid;
  place-items: center;
}
.red {
  background-color: red;
}
</style>
