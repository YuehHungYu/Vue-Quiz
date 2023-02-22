<template>
  <div>
    <b-nav tabs>
      <b-nav-item disable> <b>Quiz App</b></b-nav-item>
      <b-nav-item disable>{{ numCorrect }}/{{ numTotal }}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default{
  props:[
    'numCorrect',
    'numTotal'
  ]
}
</script>