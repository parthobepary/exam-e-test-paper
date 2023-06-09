<template>
    <div>
        <ExamComponent :data="items"/>
    </div>
</template>
<script setup>
const items = ref([]);


const route = useRoute();
const subject_id = route?.query.subject_id;
const type = route?.query.type;
const fbcl_id = route?.query.fbclid;
const year = route?.params.year;
const id = route?.params.id;
const question = route?.params.question;
const isLoading = ref(false);

//function

const init = async () => {
    console.log('jjjj');
    isLoading.value = true;
    const { data, pending, error } = await useFetch(`https://api.e-testpaper.com/api/boards/${id}/${question}/${year}/?subject_id=${subject_id}&type=${type}&fbclid=${fbcl_id}`)
    if (error && error._value) {
        console.log(error);
    } else {
        console.log(data.value);
        items.value = data.value.questions;
    }
    isLoading.value = false;
}
init();

</script>
<style scoped></style>