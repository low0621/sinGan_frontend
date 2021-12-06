<script>
export let value = {
  'image1': '',
  'image2': '',
  'image3':'',
  'scale': '',
  'path':''
}
let source, target,mask

$: if(source && target&&mask) {
  value.image1 = source[0].name
  value.image2 = target[0].name
  value.image3 = mask[0].name
}
</script>

<template lang='pug'>

<!--  input.mb-4(name='source' type='file' bind:files='{source}')-->
<!--  input.mb-4(name='target' type='file' bind:files='{target}')-->
.inline-block.content-centers
  .inline-block.content-centers
    .mb-5.content-centers
      img.border-solid.border-4.border-light-blue-500.object-contain.h-40.w-40(id="img1" src="#" alt="img1_name")
    label.mx-6.my-3.w-50.px-5.py-2.bg-white.rounded-md.shadow-md.tracking-wide.uppercase.border.border-blue.cursor-pointer.text-blue-600.ease-linear.transition-all.duration-150(class="hover:bg-blue-600 hover:text-white")
      i.fas.fa-cloud-upload-alt.fa-3x 
      span.mt-4.text-base.leading-normal image1
      input.hidden(type='file' bind:files='{source}' onchange="document.getElementById('img1').src = window.URL.createObjectURL(this.files[0])")
  .inline-block.content-centers.mx-5
    .mb-5
      img.border-solid.border-4.border-light-blue-500.object-contain.h-40.w-40(id="img2" src="#" alt="img2_name")
    label.mx-6.items-center.my-3.w-50.px-5.py-2.bg-white.rounded-md.shadow-md.tracking-wide.uppercase.border.border-blue.cursor-pointer.text-blue-600.ease-linear.transition-all.duration-150(class="hover:bg-blue-600 hover:text-white")
      i.fas.fa-cloud-upload-alt.fa-3x 
      span.mt-2.text-base.leading-normal image2
      input.hidden(type='file' bind:files='{target}' onchange="document.getElementById('img2').src = window.URL.createObjectURL(this.files[0])")
  .inline-block.content-centers
    .mb-5
      img.border-solid.border-4.border-light-blue-500.object-contain.h-40.w-40(id="img3" src="#" alt="img3_name")
    label.mx-6.items-center.my-3.w-50.px-5.py-2.bg-white.rounded-md.shadow-md.tracking-wide.uppercase.border.border-blue.cursor-pointer.text-blue-600.ease-linear.transition-all.duration-150(class="hover:bg-blue-600 hover:text-white")
      i.fas.fa-cloud-upload-alt.fa-3x 
      span.mt-2.text-base.leading-normal image3
      input.hidden(type='file' bind:files='{mask}' onchange="document.getElementById('img3').src = window.URL.createObjectURL(this.files[0])")
.inline-block.content-centers
  span.mr-2 Scale
  input.mt-4.mb-2.px-4.py-2.rounded-lg.border.border-gray-300.text-xs.w-20(type='number' class='focus:outline-none focus:ring-2 focus:ring-gray-200' bind:value='{value.scale}')
.inline-block.content-centers
  span.mr-2 Path
  input.mb-4.px-4.py-2.rounded-lg.border.border-gray-300.text-xs.w-30(type='text' bind:value='{value.path}' class='focus:outline-none focus:ring-2 focus:ring-gray-200')
</template>

<style>
</style>

