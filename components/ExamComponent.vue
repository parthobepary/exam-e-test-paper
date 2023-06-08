<template>
    <div>
        <div class="text-center d-flex pb-4">
        </div>
        <v-expansion-panels multiple>
            <v-expansion-panel v-for="id, i in data" :key="1" :value="removeP(id.question)" ripple>
                <div>
                    <v-expansion-panel-title expand-icon="mdi-menu-down">
                        <p>{{ i + 1 + '.' }} <span v-katex="removeP(id.question )" class="latex"></span></p>
                        <template v-slot:actions="{ expanded }">
                            <Icon :name="expanded ? 'codicon:chevron-down' : 'codicon:chevron-up'" color="black" />
                        </template>
                    </v-expansion-panel-title>
                    <div v-if="!getUserRes(id._id)">
                        <v-expansion-panel-text v-model="panel" v-for="t, i in id.options" :key="i">
                            <v-btn @click="answerQuestion(t, id)">{{ i }} {{ t }}</v-btn>
                        </v-expansion-panel-text>
                    </div>
                    <div v-else>
                        <v-expansion-panel-text v-model="panel" v-for="t, i in id.options" :key="i">
                            <div :class="id.answer == t ? 'correct_ans' : getUserRes(id._id).ua == t ? 'wrong_ans' : 'user_ans'"
                            
                                class="d-flex justify-space-between">
                                <p>{{ i }} {{ t }}</p>
                                <p>
                                    <icon
                                        :name="id.answer == t ? 'heroicons-solid:check-circle' : getUserRes(id._id).ua == t ? 'ic:baseline-cancel' : ''" />
                                </p>
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
const isShowExplain = ref({});


const props = defineProps({
    data: {
        type: Array,
        default: []
    }
});



// function

const removeP = (item) => {
    let pera = item?.replace('p', 'span');
    return pera;
}
const hideExplain = () => {
    isShowExplain.value = !isShowExplain.value;
};

const getDigit = (i) => {
    if (i == 0) return 'a . ';
    else if (i == 1) return 'b . ';
    else if (i == 2) return 'c . ';
    else return 'd . '
};
const showExplain = () => {
    isShowExplain.value = !isShowExplain.value;
};
const answerQuestion = (t, id) => {
    ua.value = id
    const ans = {
        re: id,
        ua: t
    }
    userAnswer.value.push(ans);
};

const getUserRes = (id) => {
    return userAnswer.value.find(u => u.re._id == id);
    // console.log(id);
}
</script>
  
<style scoped>
.user_ans {
    background-color: yellow !important;
}

.wrong_ans {
    background-color: red !important;
}

.correct_ans {
    background-color: blue !important;
}
</style>