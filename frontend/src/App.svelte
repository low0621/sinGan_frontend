<script>
import { onMount } from 'svelte'
import ThreeInputs from './component/ThreeInputs.svelte'
import SuperResolution from './component/SuperResolution.svelte'
import Animation from './component/Animation.svelte'
import Paint2Image from './component/Paint2Image.svelte'

let currentMode = 'Paint2Image'
let components = {
  "Paint2Image": Paint2Image,
  "Editing": ThreeInputs,
  "Harmonization": ThreeInputs,
  "SuperResolution": SuperResolution,
  "Animation": Animation
}
let modes = ['Paint2Image', 'Editing', 'Harmonization', 'SuperResolution', 'Animation']
let progress
let status
let initial
let props
let url
let stoplog
let serverURL = `${import.meta.env.VITE_SERVER_URL}:${import.meta.env.VITE_SERVER_PORT}`


const submit = () => {
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


const stop = () => {
  fetch(`${serverURL}/stop`)
  .then(res => {
    return res.text()
  })
  .then(result => {
    stoplog = result
    console.log(stoplog)
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
    status = result
  })
}

const getIni = () => {
  fetch(`${serverURL}/initial`)
  .then(res => {
    return res.text()
  })
  .then(result => {
    initial = result
  })
}


onMount(async () => {
  getIni()
  getProgress()
  getStatus()
  const interval = setInterval(function() {getProgress()}, 1000)
  const interval2 = setInterval(function() {getStatus()}, 1000)
})



</script>

<template lang='pug'>
.h-screen.w-screen.flex.items-center.justify-center
  .flex.flex-col.items-center(class='w-4/6')
    .mb-4.flex.flex-col.justify-center.items-center.w-full
      +if('progress != "100%" && progress != "-1%"')
        .relative.pt-1.w-full.text-center {progress}
          .overflow-hidden.h-2.mb-4.text-xs.text-center.flex.rounded.bg-blue-200
            .transition-width.duration-300.shadow-none.flex.flex-col.text-center.whitespace-nowrap.text-white.justify-center.bg-blue-500(style='width: {progress}')
        +elseif('progress == "-1%"')
          div Ready for training!!
        +elseif('progress == "100%"')
          div Training complete!!
        +elseif('status == "done"')
          div Output complete!!
    .w-full.flex.flex-col.items-center(class!='{status != "done" && status != "ready"? "opacity-50 pointer-events-none" : ""}')
      .inline-flex.justify-between.mb-4.w-full
        +each('modes as mode')
          input.appearance-none.cursor-pointer.border.border-blue-400.flex.justify-center.py-2.mr-2(class='last:mr-0 w-1/5 hover:bg-blue-400 hover:text-white' class!='{currentMode == mode ? "bg-blue-400 text-white" : ""}' type='radio' name='mode' on:click!='{() => currentMode = mode}' label='{mode}' checked='{currentMode == mode}')
          //- .inline-flex.items-center.mr-4
          //-   input.mr-1(type='radio' name='mode' on:click!='{() => currentMode = mode}' value='{mode}')
      svelte:component(this='{components[currentMode]}' bind:value='{props}')
      button.bg-blue-500.text-white.font-bold.py-2.px-4(class='w-28 hover:bg-blue-700 rounded' on:click='{submit}') Submit
    button.my-3.bg-blue-500.text-white.font-bold.py-2.px-4(class='w-28 hover:bg-blue-700 rounded' on:click='{stop}') Stop
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
</style>
