<!-- <template>
    <div>
        <div v-if="isLoading">
            <div class="d-flex justify-center align-center h-screen">
                <p>Loading ...</p>
            </div>
        </div>
        <div v-else>


        </div>
    </div>
</template>
<!-->

<template>
    <div>
        <div v-if="isLoading">
            <div class="d-flex justify-center align-center h-screen">
                <p>Loading ...</p>
            </div>
        </div>
        <div v-else>
            <div class="sticky d-flex justify-space-between btn-body">
                <div style="padding: 5px 0px; width: 100%; display: flex; flex-direction: column; justify-content: center; align-items: center;"
                    class="first-btn" :class="!isActive ? 'active' : ''">
                    <button @click="first">জ্ঞান ও অনুধাবন</button>
                    <div v-if="!isActive" style="width: 40px; height: 3px; background-color: #2E3192; margin: 3px 0px;">
                    </div>
                </div>
                <div style="padding: 5px 0px; width: 100%; display: flex; flex-direction: column; justify-content: center; align-items: center;"
                    class="first-btn" :class="isActive ? 'active' : ''">
                    <button @click="second">প্রয়োগ ও উচ্চতর দক্ষতা</button>
                    <div v-if="isActive" style="width: 40px; height: 3px; background-color: #2E3192; margin: 3px; 0px">
                    </div>
                </div>
            </div>
            <div>
                <div style="background-color: #F3F4FA;">
                    <div style="padding: 25px 20px;">
                        <div v-if="question && question.length">
                            <ExamComponent :data="question" />
                        </div>
                        <div v-else>
                            <p class="px-2 text-center mt-5 py-3">No question here</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>

const items = ref([]);
const question = ref([]);
const isActive = ref(true);
const route = useRoute();
const topic_id = route?.query.topic;
const fbcl_id = route?.query.fbclid;
const isLoading = ref(false);

// const checkData = [
//     {
//         name:'hello',
//         type:'a'
//     },
//     {
//         name:'hello',
//         type:'a'
//     },
//     {
//         name:'hello',
//         type:'b'
//     },
//     {
//         name:'hello',
//         type:'b'
//     },
//     {
//         name:'hello',
//         type:'c'
//     },
//     {
//         name:'hello',
//         type:'c'
//     },
//     {
//         name:'hello',
//         type:'d'
//     },
//     {
//         name:'hello',
//         type:'d'
//     },
// ]

// console.log(topic_id, fbcl_id);

// 64798aa4330fd9518a1783df
//IwAR3s43hdmzd3ByT7Y59ZHeQXBICSFlfbtZDKgPIHwP8dKEl_Ek_TM3tkgo4




const init = async () => {
    isLoading.value = true;
    const { data, pending, error } = await useFetch(`https://api.e-testpaper.com/api/mcq?topic=${topic_id}&fbclid=${fbcl_id}`)
    if (error && error._value) {
        console.log(error);
    } else {
        items.value = data.value.mcqs;
        first();
    }
    isLoading.value = false;
}
init();

const first = () => {
    question.value = items.value.filter(mc => {
        return (mc.type == 'a' || mc.type == 'b')
    });
    isActive.value = false;
}
const second = () => {
    question.value = items.value.filter(mc => {
        return (mc.type == 'c' || mc.type == 'd')
    });
    isActive.value = true;
}

</script>

<style scoped>
.btn-body {
    background: #CDDDE7;
    margin: 10px 20px;
    border-radius: 10px;
    padding: 10px 5px;
    position: static;
}

.active {
    background-color: white;
}

.first-btn {
    border-radius: 10px;
}


.second-btn {
    text-align: center;
    width: 100%;
    padding: 10px 0px;
    border-radius: 10px;
}

.sticky {
    position: -webkit-sticky;
    position: sticky;
    top: 0;
    z-index: 199;
}
</style>