<script>
import SourceUpload from './SourceUpload.svelte'
import ImageUpload from './ImageUpload.svelte'

export let props
export let maxScale

let source, target, mask

$: if (source && target && mask){
  props = {
    image1: source[0].name,
    image2: target[0].name,
    image3: mask[0].name
  }
}

</script>
<template lang='pug'>
.flex.flex-col.h-full.mr-6.text-purple(class='w-1/3')
  svelte:component(this='{SourceUpload}' bind:file='{source}' bind:maxScale='{maxScale}')
.flex.flex-col.h-full.mr-6.text-purple(class='w-1/3')
  svelte:component(this='{ImageUpload}' bind:file='{target}' imageType='Target')
.flex.flex-col.h-full.text-purple(class='w-1/3')
  svelte:component(this='{ImageUpload}' bind:file='{mask}' imageType='Mask')
</template>
<style>
@tailwind base;
@tailwind components;
@tailwind utilities;
</style>
