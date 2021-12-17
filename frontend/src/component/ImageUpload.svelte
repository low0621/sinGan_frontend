<script>
export let file
export let imageType
let preview


const imageChange = e => {
  if (!file.length) {
    preview.style.backgroundImage = ''
    return
  }

  let src = window.URL.createObjectURL(e.target.files[0])
  preview.style.backgroundImage = `url(${src})`
}
</script>
<template lang='pug'>
.mb-2.eras {imageType}
.bg-contain.bg-no-repeat.bg-center.rounded.border.border-purple-300.mb-2(bind:this='{preview}' class='h-3/4')
label.JhengHei.rounded.border.border-purple.flex.items-center.justify-center.cursor-pointer(class='h-1/4 hover:bg-purple hover:text-white') 選擇檔案
  input.hidden(type='file' bind:files='{file}' on:change!='{imageChange}')
</template>
<style>
@tailwind base;
@tailwind components;
@tailwind utilities;

.eras {
  font-family: eras;
}

.JhengHei {
  font-family: Microsoft JhengHei;
}
</style>
