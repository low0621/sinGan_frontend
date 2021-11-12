<script>
export let value = {
  'image1': '',
  'resolution': ''
}
let source

$: if (source) value.image1 = source[0].name
</script>

<template lang='pug'>
.inline-block
  input.mb-4(type='file' bind:files='{source}')
.inline-block
  span.mr-2 Resolution
  input.mb-4.px-4.py-2.rounded-lg.border.border-gray-300.text-xs.w-20(type='number' bind:value='{value.resolution}' class='focus:outline-none focus:ring-2 focus:ring-gray-200')
</template>

<style>
</style>

