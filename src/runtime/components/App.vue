<script lang="ts">
import type { ConfigProviderProps, TooltipProviderProps } from 'reka-ui'
import type { ToasterProps } from '../types'

export interface AppProps extends ConfigProviderProps {
  tooltip?: TooltipProviderProps
  toaster?: ToasterProps | null
}

export interface AppSlors {
  default(props?: {}): any
}
</script>

<script setup lang="ts">
import { toRef } from 'vue'
import { ConfigProvider, TooltipProvider, useForwardProps } from 'reka-ui'
import { reactivePick } from '@vueuse/core'
import { useId } from '#imports'
import UToaster from './Toaster.vue'
import UModalProvider from './ModalProvider.vue'
import USlideoverProvider from './SlideoverProvider.vue'

const props = withDefaults(defineProps<AppProps>(), {
  useId: () => useId()
})
defineSlots<AppSlors>()

const configProviderProps = useForwardProps(reactivePick(props, 'dir', 'scrollBody', 'useId'))
const tooltipProps = toRef(() => props.tooltip)
const toasterProps = toRef(() => props.toaster)
</script>

<template>
  <ConfigProvider v-bind="configProviderProps">
    <TooltipProvider v-bind="tooltipProps">
      <UToaster v-if="toaster !== null" v-bind="toasterProps">
        <slot />
      </UToaster>
    </TooltipProvider>

    <UModalProvider />
    <USlideoverProvider />
  </ConfigProvider>
</template>
