<script lang="ts" setup>
import { Accelerator } from "@packages/share";
import { ref } from "vue";
const { electronBridge } = window;

const id = ref<number | null>(null);
const accelerator: Accelerator.Pattern = `${Accelerator.Modifier.Ctrl}+${Accelerator.Modifier.Shift}+${Accelerator.Key.KeyA}`;

electronBridge.electron.on("gs", (...args: any[]) => {
  console.log("List message", args);
});

async function onClick() {
  const callback = async (event: any, accelerator: string) => {
    console.log(event, accelerator);
    console.log("CurrentProcessId", id);
  };
  const registered = await electronBridge.globalShortcut.register(
    accelerator,
    callback
  );
  console.log("registered", registered);
}
</script>

<template>
  {{ id }}
  <ul>
    <li>chrome version: {{ electronBridge.versions.chrome() }}</li>
    <li>node version: {{ electronBridge.versions.node() }}</li>
    <li>electron version: {{ electronBridge.versions.electron() }}</li>
  </ul>
  <button @click="onClick">getCurrentProcessId</button>
</template>
