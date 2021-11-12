<script>
import { onMount } from 'svelte'
import ThreeInputs from './component/ThreeInputs.svelte'
import SuperResolution from './component/SuperResolution.svelte'
import Animation from './component/Animation.svelte'

let currentMode = ''
let components = {
  "Paint2Image": ThreeInputs,
  "Editing": ThreeInputs,
  "Harmonization": ThreeInputs,
  "SuperResolution": SuperResolution,
  "Animation": Animation
}
let modes = ['Paint2Image', 'Editing', 'Harmonization', 'SuperResolution', 'Animation']
let progress
let props
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

const getProgress = () => {
  fetch(`${serverURL}/progress`)
  .then(res => {
    return res.text()
  })
  .then(result => {
    progress = result
  })
}

onMount(async () => {
  getProgress()
  const interval = setInterval(function() {getProgress()}, 60000)
})

</script>

<template lang='pug'>
.h-screen.w-screen.flex.items-center.justify-center
  .flex.flex-col(class='w-3/6').items-center
    .mb-4.flex.flex-col.justify-center.items-center.w-full
      +if('progress != "100%"')
        .relative.pt-1.w-full.text-center {progress}
          .overflow-hidden.h-2.mb-4.text-xs.text-center.flex.rounded.bg-blue-200
            .transition-width.duration-300.shadow-none.flex.flex-col.text-center.whitespace-nowrap.text-white.justify-center.bg-blue-500(style='width: {progress}')
        +else()
          div Training complete!!
    +if('progress')
      .inline-flex.mb-4.justify-between.w-full
        +each('modes as mode')
          .inline-flex.items-center.mr-4
            input.mr-1(type='radio' name='mode' on:click!='{() => currentMode = mode}' value='{mode}')
            span {mode}
      .flex.flex-col.w-full
        svelte:component(this='{components[currentMode]}' bind:value='{props}')
      button.bg-blue-500.text-white.font-bold.py-2.px-4(class='w-28 hover:bg-blue-700 rounded' on:click='{submit}') Submit
</template>

<style>
@tailwind base;
@tailwind components;
@tailwind utilities;
</style>
