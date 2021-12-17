<script>
export let imageType
</script>
<template lang='pug'>
.mb-2.eras {imageType}
.rounded.border.border-purple-300.mb-2(class='h-3/4')
label.JhengHei.rounded.border.border-purple.flex.items-center.justify-center(class='h-1/4') 選擇檔案
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
