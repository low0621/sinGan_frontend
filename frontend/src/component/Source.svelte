<script>
import SourceUpload from './SourceUpload.svelte'
import DisableUpload from './DisableUpload.svelte'

export let props
export let maxScale

let source

$: if (source && target){
  props = {
    image1: source,
  }
}

</script>
<template lang='pug'>
.flex.flex-col.h-full.mr-6.text-purple(class='w-1/3')
  svelte:component(this='{SourceUpload}' bind:file='{source}' bind:maxScale='{maxScale}')
.flex.flex-col.h-full.mr-6.text-purple.opacity-30(class='w-1/3')
  svelte:component(this='{DisableUpload}' imageType='Target')
.flex.flex-col.h-full.text-purple.opacity-30(class='w-1/3')
  svelte:component(this='{DisableUpload}' imageType='Mask')
</template>
<style>
@tailwind base;
@tailwind components;
@tailwind utilities;
</style>
