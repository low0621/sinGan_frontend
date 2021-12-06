<script>
export let value = {
  'image1': '',
  'resolution': '',
  'path':''
}
let source

$: if (source) value.image1 = source[0].name
</script>

<template lang='pug'>
//- .inline-block
//- <!-- input.mb-4(type='file' bind:files='{source}') -->
.inline-block.content-centers.mb-4
  .flex.justify-center.mb-5.content-centers
    img.border-solid.border-4.border-light-blue-500.object-contain.h-40.w-40(id="img1" src="src/assets/noimage.gif" alt="img1_name")
  label.mx-10.my-3.w-50.px-5.py-2.bg-white.rounded-md.shadow-md.tracking-wide.uppercase.border.border-blue.cursor-pointer.text-blue-600.ease-linear.transition-all.duration-150(class="hover:bg-blue-600 hover:text-white")
    i.fas.fa-cloud-upload-alt.fa-3x
    span.mt-2.text-base.leading-normal Source
    input.hidden(type='file' bind:files='{source}' onchange="document.getElementById('img1').src = window.URL.createObjectURL(this.files[0])")
.flex.flex-col.items-center.content-centers
  span Resolution
  input.my-2.px-4.py-2.rounded-lg.border.border-gray-300.text-xs.w-20.text-center(placeholder='Resolution' type='number' bind:value='{value.resolution}' class='focus:outline-none focus:ring-2 focus:ring-gray-200')
.flex.flex-col.items-center.content-centers
  span Path
  input.mt-2.mb-4.px-4.py-2.rounded-lg.border.border-gray-300.text-xs.w-30.text-center(placeholder='Path' type='text' bind:value='{value.path}' class='focus:outline-none focus:ring-2 focus:ring-gray-200')
</template>

<style>
</style>

