<template>
    <div>
        <v-expansion-panels multiple>
            <v-expansion-panel elevation="0"
                style="margin-bottom: 15px !important; border-radius: 10px !important;" v-for="id, i in data" :key="i"
                :value="removeP(id.question)">
                <div>
                    <v-expansion-panel-title expand-icon="mdi-menu-down" class="px-0 mx-0"
                        style="background-color: white !important; border-radius: 10px !important; padding: 10px !important;">
                        <p>{{ i + 1 + '.' }} <span style="line-height: 1.4rem;" v-katex="removeP(id.question)"
                                class="latex"></span></p>
                        <template v-slot:actions="{ expanded }">
                            <img :src="expanded ? '/images/u.svg' : '/images/d.svg'" alt="">
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
                            <div v-for="t, i in id.options" :key="i" class="mb-2"
                                :class="id.answer == t ? 'correct_ans' : getUserRes(id._id).ua == t ? 'wrong_ans' : 'user_ans'">
                                <div class="d-flex justify-space-between align-center" style="padding: 0px 10px;">
                                    <p>{{ getDigit(i) }} <span v-katex="t"></span></p>
                                    <p>
                                        <icon size="30"
                                            :color="id.answer == t ? '#31CB18' : getUserRes(id._id).ua == t ? 'red' : ''"
                                            :name="id.answer == t ? getUserRes(id._id).ua == t ? 'heroicons-solid:check-circle' : '' : getUserRes(id._id).ua == t ? 'ic:baseline-cancel' : ''" />
                                    </p>
                                </div>
                            </div>
                            <div v-if="id.explain && id.explain.length" class="">
                                <div style="display: flex; justify-content: center;">
                                    <button v-if="!getExplain(id._id)"
                                        style="color: #045689; border: 1px solid #045689; padding: 2px 15px; border-radius: 8px; margin: 10px 0px;"
                                        @click="showExplain(id)" class="">See Explanation</button>
                                </div>
                                <div v-if="getExplain(id._id)" class="explain">
                                    <p><span style="color: #045689;">Explanation : </span><span
                                            v-katex="removeP(getExplain(id._id).explain)"></span></p>
                                </div>
                                <div v-if="getExplain(id._id)" style="display: flex; justify-content: center;">
                                    <button @click="hideExplain(id)"
                                        style="color: #045689; border: 1px solid #045689; padding: 2px 15px; border-radius: 8px; margin: 10px 0px;">Hide
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
const isHide = ref([])


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
const showExplain = (id) => {
    isShowExplain.value = !isShowExplain.value;
    isHide.value.push(id);
    getExplain(id)
};
const hideExplain = (id) => {
    isShowExplain.value = !isShowExplain.value;
    const index = isHide.value.indexOf(id);
    if (index == 0) {
        isHide.value.shift();
    } else {
        isHide.value.splice(index, 1);
    }
    console.log(index);
    getExplain(id)
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
const getExplain = (id) => {
    return isHide.value.find(u => u._id == id)
}
</script>
  
<style lang="scss" scoped>
.user_ans {
    background-color: #F3F4FA !important;
    padding: 7.5px 2px;
    border-radius: 10px;
}

.wrong_ans {
    background-color: #FBEDEA !important;
    padding: 7.5px 2px;
    border-radius: 10px;
    border: 1px solid #ED4E2B;
}

.correct_ans {
    background-color: #EAF7EE !important;
    padding: 7.5px 2px;
    border: 1px solid #009456;
    border-radius: 10px;
}

.explain {
    background: rgba(247, 148, 29, 0.2);
    border-radius: 8px;
    padding: 10px;
    margin-bottom: 7px;
}

// vuetify overwrite css

.gradient-text {
    /* Fallback: Set a background color. */
    background-color: red;

    /* Create the gradient. */
    background-image: linear-gradient(45deg, #f3ec78, #af4261);

    /* Set the background size and repeat properties. */
    background-size: 100%;
    background-repeat: repeat;

    /* Use the text as a mask for the background. */
    /* This will show the gradient as a text color rather than element bg. */
    background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}

:deep() {
    .v-expansion-panel:not(:first-child)::after {
    border-top-style: none !important; 
    border-top-width: none !important; 
    content: "";
    left: 0;
    position: absolute;
    right: 0;
    top: 0;
    transition: 0.3s opacity cubic-bezier(0.4, 0, 0.2, 1) !important; 
}
    .v-expansion-panel-title__overlay {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: white !important;
        border-radius: inherit;
        opacity: 0;
    }

    .v-expansion-panel-text__wrapper {
        padding: 0px 10px 10px 10px !important;
        flex: 1 1 auto;
        max-width: 100%;
    }

}
</style>