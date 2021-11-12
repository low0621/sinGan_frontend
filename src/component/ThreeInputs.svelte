<script>
export let value = {
  'image1': '',
  'image2': '',
  'scale': '',
}
let source, target

$: if(source && target) {
  value.image1 = source[0].name
  value.image2 = target[0].name
}
</script>

<template lang='pug'>
.inline-block
  span.mr-2 Scale
  input.mb-4.px-4.py-2.rounded-lg.border.border-gray-300.text-xs.w-20(type='number' class='focus:outline-none focus:ring-2 focus:ring-gray-200' bind:value='{value.scale}')
.inline-block
  input.mb-4(name='source' type='file' bind:files='{source}')
  input.mb-4(name='target' type='file' bind:files='{target}')
</template>

<style>
</style>

