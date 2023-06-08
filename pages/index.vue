<template>
  <div>
    <div class="text-center d-flex pb-4">
    </div>
    <v-expansion-panels multiple>
      <v-expansion-panel v-for="id, i in data" :key="1" :value="id.name" ripple>
        <div>
          <v-expansion-panel-title expand-icon="mdi-menu-down">
            {{ id.name }}
            <template v-slot:actions="{ expanded }">
              <Icon :name="expanded ? 'codicon:chevron-down' : 'codicon:chevron-up'" color="black" />
            </template>
          </v-expansion-panel-title>
          <div v-if="!getUserRes(id.id)">
            <v-expansion-panel-text v-model="panel" v-for="t, i in id.option" :key="i">
              <v-btn @click="answerQuestion(t, id)">{{ i }} {{ t }}</v-btn>
            </v-expansion-panel-text>
          </div>
          <div v-else>
            <v-expansion-panel-text v-model="panel" v-for="t, i in id.option" :key="i">
              <div :class="id.ans == t ? 'correct_ans' : getUserRes(id.id).ua == t ? 'wrong_ans' : 'user_ans'" class="d-flex justify-space-between">
                <p>{{ i }} {{ t }}</p>
                <p><icon :name="id.ans == t ? 'heroicons-solid:check-circle' : getUserRes(id.id).ua == t ? 'ic:baseline-cancel' : ''" /></p>
              </div>
            </v-expansion-panel-text>
          </div>
        </div>
      </v-expansion-panel>
    </v-expansion-panels>
  </div>
</template>

<script setup>

const panel = ref([]);
const userAnswer = ref([]);
const ua = ref({});

const props = defineProps({
  data: {
    type: Array,
    default: [
      {
        id: 1,
        name: 'test 3',
        des: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore',
        option: [
          'hello 1',
          'hi 2',
          'oh'
        ],
        ans:'hello 1'
      },
      {
        id: 2,
        name: 'test 2',
        des: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore',
        option: [
          'hello 1',
          'hi 2',
          'oh'
        ],
        ans:'hi 2'
      },
      {
        id: 3,
        name: 'test 1',
        des: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore',
        option: [
          'hello 1',
          'hi 2',
          'oh'
        ],
        ans:'oh'
      },

    ]
  }
})


const answerQuestion = (t, id) => {
  ua.value = id
  const ans = {
    re: id,
    ua: t
  }
  userAnswer.value.push(ans);
};

const getUserRes = (id) => {
  return userAnswer.value.find(u => u.re.id == id)
}
</script>

<style scoped>
.user_ans{
  background-color: yellow !important;
}
.wrong_ans{
  background-color: red !important;
}
.correct_ans{
  background-color: blue !important;
}
</style>