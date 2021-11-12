<script>
export let value = {
  'image1': ''
}
let source

$: if (source) value.image1 = source[0].name
</script>

<template lang='pug'>
input.mb-4(type='file' bind:files='{source}')
</template>

<style>
</style>

