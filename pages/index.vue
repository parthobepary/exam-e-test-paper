<template>
  <div>
    <div class="text-center d-flex pb-4">
    </div>
    <div>
      <ExamComponent :data="data"/>
    </div>
  </div>
</template>

<script setup>

const data = [
  {
    id: 1,
    name: 'test 3',
    des: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore',
    option: [
      'hello 1',
      'hi 2',
      'oh'
    ],
    ans: 'hello 1'
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
    ans: 'hi 2'
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
    ans: 'oh'
  },

]

</script>

<style scoped></style>