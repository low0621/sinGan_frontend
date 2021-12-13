<script>
export let value = {
  'image1': '',
  'path':''
}

let source

$: if (source && source.length > 0) value.image1 = source[0].name

$: if(source && source.length > 0){
    document.getElementById('img1').src = window.URL.createObjectURL(source[0])
}

</script>

<template lang='pug'>
.inline-block.content-centers.pb-5.pt-1.mb-2
  .flex.justify-center.mb-5.content-centers
    img.border-solid.border-4.border-light-blue-500.object-contain.h-40.w-40(id="img1" src="src/assets/noimage.gif" alt="img1_name")
  label.mx-10.my-3.w-50.px-5.py-2.bg-white.rounded-md.shadow-md.tracking-wide.uppercase.border.border-blue.cursor-pointer.text-blue-600.ease-linear.transition-all.duration-150(class="hover:bg-blue-600 hover:text-white")
    i.fas.fa-cloud-upload-alt.fa-3x
    span.mt-2.text-base.leading-normal SOURCE
    input.hidden(id='a_img1' type='file' bind:files='{source}')
.flex.flex-col.items-center.content-centers
</template>



<style>
</style>

