<template>
    <div>
        <div class="text-center d-flex pb-4">
        </div>
        <v-expansion-panels multiple>
            <v-expansion-panel style="margin-bottom: 10px !important; border-radius: 10px !important;" v-for="id, i in data"
                :key="1" :value="removeP(id.question)">
                <div>
                    <v-expansion-panel-title expand-icon="mdi-menu-down"
                        style="background-color: white !important; border-radius: 10px !important;">
                        <p>{{ i + 1 + '.' }} <span style="line-height: 1.4rem;" v-katex="removeP(id.question)"
                                class="latex"></span></p>
                        <template v-slot:actions="{ expanded }">
                            <Icon :name="expanded ? 'codicon:chevron-up' : 'codicon:chevron-down'" color="#0364A6" />
                        </template>
                    </v-expansion-panel-title>
                    <div v-if="!getUserRes(id._id)">
                        <v-expansion-panel-text v-model="panel" v-for="t, i in id.options" :key="i">
                            <button
                                style="padding: 10px; border-radius: 10px; background-color: #F3F4FA !important; text-align: justify !important; width: 100% !important;"
                                @click="answerQuestion(t, id)">{{ getDigit(i) }} <span v-katex="t"></span></button>
                        </v-expansion-panel-text>
                    </div>
                    <div v-else>
                        <v-expansion-panel-text v-model="panel">
                            <div v-for="t, i in id.options" :key="i" class="my-2"
                                :class="id.answer == t ? 'correct_ans' : getUserRes(id._id).ua == t ? 'wrong_ans' : 'user_ans'">
                                <div class="d-flex justify-space-between">
                                    <p>{{ getDigit(i) }} <span v-katex="t"></span></p>
                                    <p>
                                        <icon size="30"
                                            :color="id.answer == t ? '#31CB18' : getUserRes(id._id).ua == t ? 'red' : ''"
                                            :name="id.answer == t ? getUserRes(id._id).ua == t ? 'heroicons-solid:check-circle' : '' : getUserRes(id._id).ua == t ? 'ic:baseline-cancel' : ''" />
                                    </p>
                                </div>
                            </div>
                            <div v-if="id.explain && id.explain.length" class="pl-4">
                                <div v-if="!isShowExplain" style="display: flex; justify-content: center;">
                                    <button
                                        style="color: #045689; border: 1px solid #045689; padding: 8px; border-radius: 10px; margin-bottom: 10px;"
                                        @click="showExplain" class="">See Explanation</button>
                                </div>
                                <div v-if="isShowExplain" class="bg-[#FDEAD2] px-3 rounded-md py-3">
                                    <p><span style="color: #045689;">Explain : </span><span
                                            v-katex="removeP(id.explain)"></span></p>
                                </div>
                                <div v-if="isShowExplain" style="display: flex; justify-content: center;">
                                    <button @click="hideExplain"
                                        style="color: #045689; border: 1px solid #045689; padding: 8px; border-radius: 10px; margin-bottom: 10px;">Hide
                                        Explanation</button>
                                </div>
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
const isShowExplain = ref(false);


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

const getDigit = (i) => {
    if (i == 0) return 'ক . ';
    else if (i == 1) return 'খ . ';
    else if (i == 2) return 'গ . ';
    else return 'ঘ . '
};
const showExplain = () => {
    isShowExplain.value = !isShowExplain.value;
};
const hideExplain = () => {
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
    background-color: #F3F4FA !important;
    padding: 10px 6px;
    border-radius: 10px;
}

.wrong_ans {
    background-color: rgb(245, 224, 224) !important;
    padding: 10px 6px;
    border-radius: 10px;
}

.correct_ans {
    background-color: #DDF5FF !important;
    padding: 10px 6px;
    border-radius: 10px;
}
</style>