<script>
import { onMount } from 'svelte'
import Source from './component/Source.svelte'
import SourceTarget from './component/SourceTarget.svelte'
import SourceTargetMask from './component/SourceTargetMask.svelte'

let serverURL = `${import.meta.env.VITE_SERVER_URL}:${import.meta.env.VITE_SERVER_PORT}`
let components = {
  'Paint2Image': SourceTarget,
  'Editing': SourceTargetMask,
  'Harmonization': SourceTargetMask,
  'SuperResolution': Source,
  'Animation': Source,
  'RandomSample': Source
}
let currentMode = 'Paint2Image'
let maxScale = 99
let modes = ['Paint2Image', 'Editing', 'Harmonization', 'SuperResolution', 'Animation','RandomSample']
let progress = '100%'
let source, target, mask, scale
let _status, props, width, height


const submit = () => {
  switch (currentMode) {
    case 'SuperResolution':
      props['resolution'] = scale
      break
    case 'RandomSample':
      props['width'] = width
      props['height'] = height
      break
    case 'Anmation':
      break
    default:
      props['scale'] = scale
  }
  fetch(`${serverURL}/${currentMode}`, {
    body: JSON.stringify(props),
    headers: {
      'content-type': 'application/json',
    },
    method: 'POST'
  })
  .then( res => {
    return res.json()
  })
  .then( result => {
    getProgress()
  })
}

const getProgress = () => {
  fetch(`${serverURL}/progress`)
  .then(res => {
    return res.text()
  })
  .then(result => {
    progress = result
  })
}

const getStatus = () => {
  fetch(`${serverURL}/status`)
  .then(res => {
    return res.text()
  })
  .then(result => {
    _status = result
  })
}

const stop = () => {
  fetch(`${serverURL}/stop`)
  .then(res => {
    return res.text()
  })
  .then(result => {
    stoplog = result
  })
}

const getBaseLog = (x, y) => {
  return Math.log(y) / Math.log(x);
}

const imageChange = e => {
  let src = window.URL.createObjectURL(e.target.files[0])
  let name = e.target.name
  document.getElementById(name).style.backgroundImage = `url(${src})`
  if ('source' == name) {
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
}

onMount(async () => {
  getProgress()
  getStatus()
  const interval = setInterval(function() {getProgress()}, 1000)
  const interval2 = setInterval(function() {getStatus()}, 1000)
})
</script>

<template lang='pug'>
.w-screen.h-screen.bg-white
  .h-full.box-border.py-10.mx-auto(class='w-10/12')
    .mb-8
      .text-4xl.mb-4.eras.text-blue SinGAN
      div.w-full
        .absolute.mx-auto.h-5.flex.justify-center.items-center.eras(class='w-10/12') {progress}
        .flex.items-center.justify-center.bg-purple.h-5(style='width: {progress}')
    .inline-flex.w-full(class='h-4/5')
      .flex.flex-col.mr-8(class='w-4/12')
        .w-full.mb-4.eras.text-blue(class!='{_status != "done" ? "opacity-30 pointer-events-none" : ""}') Style
        .flex.w-full.h-full
          .flex.flex-col.justify-between.mr-8(class='w-1/2' class!='{_status != "done" ? "opacity-30 pointer-events-none" : ""}')
            +each('modes as mode')
                input.eras.rounded.appearance-none.cursor-pointer.border.border-blue.flex.justify-center.py-2(class='hover:bg-blue hover:text-white' class!='{currentMode == mode ? "bg-blue text-white" : "text-blue"}' type='radio' name='mode' on:click!='{() => currentMode = mode}' label='{mode}' checked='{currentMode == mode}')
          .flex.flex-col(class='w-1/2')
            .flex.flex-col.mb-8(class!='{_status != "done" ? "opacity-30 pointer-events-none" : ""}')
              .flex.justify-between.mb-2
                .eras.text-blue(class!='{currentMode == "RandomSample" ? "opacity-30 pointer-events-none" : ""}') Scale
                input.eras.text-center.px-4.py-2.rounded-lg.border.border-blue.text-xs.w-20(type='number' class='focus:outline-none' min=1 max='{maxScale}' bind:value='{scale}' class!='{currentMode == "RandomSample" ? "opacity-30 pointer-events-none" : ""}')
              .flex.justify-between.mb-2
                .eras.text-blue(class!='{currentMode != "RandomSample" ? "opacity-30 pointer-events-none" : ""}') Width
                input.eras.text-center.px-4.py-2.rounded-lg.border.border-blue.text-xs.w-20(type='number' class='focus:outline-none' min=0.1 max=10 step=0.1 bind:value='{width}' class!='{currentMode != "RandomSample" ? "opacity-30 pointer-events-none" : ""}')
              .flex.justify-between
                .eras.text-blue(class!='{currentMode != "RandomSample" ? "opacity-30 pointer-events-none" : ""}') Height
                input.eras.text-center.px-4.py-2.rounded-lg.border.border-blue.text-xs.w-20(type='number' class='focus:outline-none' min=0.1 max=10 step=0.1 bind:value='{height}' class!='{currentMode != "RandomSample" ? "opacity-30 pointer-events-none" : ""}')
            button.eras.bg-purple.text-white.font-bold.mb-8.py-2.rounded(class!='{_status != "done" ? "opacity-30 pointer-events-none" : ""}' on:click!='{submit}') Submit
            button.eras.bg-purple.text-white.font-bold.py-2.rounded(class!='{_status == "done" ? "opacity-30 pointer-events-none" : ""}'  on:click!='{stop}') Stop
      .flex.flex-col(class='w-8/12')
        .mb-4.eras.text-purple Output
        .w-full.box-border.p-2.border.border-purple.rounded.mb-2(class='h-3/5')
          +if('_status == "done" && progress == "100%"')
            p.eras Done !!
            +elseif('_status == "done" && progress == "0%"')
              p.eras Ready for training !!
            +elseif('progress != "100%"')
              p.eras Training ...
            +elseif('progress == "100%"')
              p.eras Image processing ...
        .w-full.flex(class='h-2/5' class!='{_status != "done" ? "opacity-30 pointer-events-none" : ""}')
          svelte:component(this='{components[currentMode]}' bind:props='{props}' bind:maxScale='{maxScale}')
</template>

<style>
@tailwind base;
@tailwind components;
@tailwind utilities;

input:before {
  content: attr(label);
  display: inline-block;
  text-align: center;
}

@font-face {
  font-family: eras;
  src: url(./assets/ErasBoldITC.ttf);
}

.eras {
  font-family: eras;
}
</style>
