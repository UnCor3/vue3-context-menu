<template>
  <CtxOptions :props="{ type: 'group', ...props }" :root="root">
    <slot />
  </CtxOptions>
</template>
<script setup lang="ts">
import { getCurrentInstance, provide, inject } from "vue";
import { checkIfValid, logError } from "@/helpers";
import CtxOptions from "@/CtxOptions.vue";
import { ActionGroup } from "@/types";

const { props } = defineProps<{
  props: ActionGroup;
}>();

checkIfValid(props);

const instance = getCurrentInstance();

const root = instance?.parent?.type.__name == "CtxAnimated";
const isInsideAGroup = inject("root", false);

if (isInsideAGroup) {
  logError(
    `You are trying to nest a ContextGroup inside another ContextGroup with label: ${props.label}. This is currently not supported, will be fixed in future versions`
  );
}

provide("root", !!root);
</script>
