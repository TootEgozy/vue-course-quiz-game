<template>
  <div>Win Count: {{ winCount }}</div>
  <div>Lose Count: {{ loseCount }}</div>
</template>

<script>
export default {
  props: ['winCount', 'loseCount']
}
</script>

<style lang="scss">

</style>
