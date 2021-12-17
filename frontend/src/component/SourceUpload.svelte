<script>
export let file
export let maxScale
let preview

const getBaseLog = (x, y) => {
  return Math.log(y) / Math.log(x);
}

const imageChange = e => {
  if (!file.length) {
    preview.style.backgroundImage = ''
    return
  }

  let src = window.URL.createObjectURL(e.target.files[0])
  preview.style.backgroundImage = `url(${src})`
  let img = new Image()
  img.onload = function() {
    let w = img.width
    let h = img.height
    let scaleFactor = 0.75
    let minSize = 20
    let maxSize = 250
    let numScale = Math.ceil(getBaseLog(scaleFactor,minSize/Math.min(w,h)))+1
    let scale2stop = Math.ceil(getBaseLog(scaleFactor,Math.min(maxSize,Math.max(w,h))/Math.max(w,h)))
    let stopScale = numScale - scale2stop
    maxScale = stopScale
  }
  img.src = src
}
</script>
<template lang='pug'>
.mb-2.eras Source
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
