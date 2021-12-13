<script>
export let value = {
  'image1': '',
  'image2': '',
  'image3':'',
  'scale': '',
  'path':''
}
let source, target,mask
let img_name
let w,h
let num_scale,scale2stop,scale_factor,min_size,max_size,stopscale

function getBaseLog(x, y) {
  return Math.log(y) / Math.log(x);
}

$: if(source && target&&mask && target.length > 0 && source.length > 0 && mask.length > 0) {
  value.image1 = source[0].name
  value.image2 = target[0].name
  value.image3 = mask[0].name
}

$: if(source) {
  document.getElementById('img1').src = window.URL.createObjectURL(source[0])
  img1.onload = () => {
    w = document.getElementById('img1').naturalWidth
    h = document.getElementById('img1').naturalHeight
    scale_factor=0.75
    min_size = 20
    max_size = 250
    num_scale = Math.ceil(getBaseLog(scale_factor,min_size/Math.min(w,h)))+1
    scale2stop = Math.ceil(getBaseLog(scale_factor,Math.min(max_size,Math.max(w,h))/Math.max(w,h)))
    stopscale = num_scale - scale2stop
    console.log(w)
    console.log(h)
    console.log("stopscale = ",stopscale)
    document.getElementById('scalenum').max = stopscale
  };
}

$: if(target && target.length > 0){
    document.getElementById('img2').src = window.URL.createObjectURL(target[0])
}

$: if(mask && mask.length > 0){
    document.getElementById('img3').src = window.URL.createObjectURL(mask[0])
}
    
</script>

<template lang='pug'>
.inline-block.content-centers.mb-4
  .inline-block.content-centers
    .mb-5.content-centers
      img.border-solid.border-4.border-light-blue-500.object-contain.h-40.w-40(id="img1" src="src/assets/noimage.gif" alt="img1_name")
    label.mx-7.my-3.w-50.px-5.py-2.bg-white.rounded-md.shadow-md.tracking-wide.uppercase.border.border-blue.cursor-pointer.text-blue-600.ease-linear.transition-all.duration-150(class="hover:bg-blue-600 hover:text-white")
      i.fas.fa-cloud-upload-alt.fa-3x
      span.mt-4.text-base.leading-normal Source
      input.hidden(type='file' id='input_train' bind:files='{source}')
  .inline-block.content-centers.mx-5
    .mb-5
      img.border-solid.border-4.border-light-blue-500.object-contain.h-40.w-40(id="img2" src="src/assets/noimage.gif" alt="img2_name")
    label.mx-8.items-center.my-3.w-50.px-5.py-2.bg-white.rounded-md.shadow-md.tracking-wide.uppercase.border.border-blue.cursor-pointer.text-blue-600.ease-linear.transition-all.duration-150(class="hover:bg-blue-600 hover:text-white")
      i.fas.fa-cloud-upload-alt.fa-3x
      span.mt-2.text-base.leading-normal Target
      input.hidden(type='file' bind:files='{target}')
  .inline-block.content-centers
    .mb-5
      img.border-solid.border-4.border-light-blue-500.object-contain.h-40.w-40(id="img3" src="src/assets/noimage.gif" alt="img3_name")
    label.mx-9.items-center.my-3.px-5.py-2.bg-white.rounded-md.shadow-md.tracking-wide.uppercase.border.border-blue.cursor-pointer.text-blue-600.ease-linear.transition-all.duration-150(class="hover:bg-blue-600 hover:text-white")
      i.fas.fa-cloud-upload-alt.fa-3x
      span.mt-2.text-base.leading-normal Mask
      input.hidden(type='file' bind:files='{mask}')
.flex.flex-col.items-center.content-centers
  span Scale
  input.text-center.my-2.px-4.py-2.rounded-lg.border.border-gray-300.text-xs.w-20(type='number' id='scalenum' class='focus:outline-none focus:ring-2 focus:ring-gray-200' bind:value='{value.scale}' min=1 max=99)
.flex.flex-col.items-center.content-centers
</template>
<style>
</style>

