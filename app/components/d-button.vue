<script setup lang="ts">
import { LoaderCircleIcon } from "lucide-vue-next"
import { NuxtLink } from "#components"

const slots = useSlots()

interface Props {
  variant?: "primary" | "secondary" | "tertiary" | "danger" | "danger-light" | "transparent" | "outline"
  iconLeft?: Component
  to?: any
  size?: "XS" | "SM" | "MD" | "LG"
  type?: "submit" | "button"
  loading?: boolean
  disabled?: boolean
}

const { variant = "primary", size = "SM", type = "button", loading = false } = defineProps<Props>()

const variantClasses: { [key: string]: string } = {
  primary: "bg-zinc-900 text-zinc-50 hover:bg-zinc-700 active:bg-zinc-800",
  secondary: "bg-zinc-100 text-zinc-700 hover:bg-zinc-200 active:bg-zinc-300",
  tertiary: "bg-emerald-800 text-emerald-50 hover:bg-emerald-700",
  danger: "bg-red-700 text-white hover:bg-red-600",
  "danger-light": "text-red-700 bg-red-100 hover:bg-red-200 active:bg-red-300",
  transparent: "text-zinc-700 hover:bg-zinc-100",
  outline: "text-zinc-600 border border-zinc-200 hover:bg-zinc-50",
}

const paddingClasses: { [key: string]: string } = {
  XS: "px-2",
  SM: "px-3",
  MD: "px-3",
  LG: "px-4",
}

const heightClasses: { [key: string]: string } = {
  XS: "h-6",
  SM: "h-7",
  MD: "h-8",
  LG: "h-9",
}

const widthClasses: { [key: string]: string } = {
  XS: "w-5",
  SM: "w-7",
  MD: "w-8",
  LG: "w-9",
}

const sizeClass = computed(() => {
  if (slots.default) {
    return [paddingClasses[size], heightClasses[size], "w-fit"]
  } else {
    return [heightClasses[size], widthClasses[size]]
  }
})
</script>

<template>
  <component
    :is="to ? NuxtLink : 'button'"
    :type
    :to
    class="relative flex min-w-7 cursor-default items-center justify-center gap-2 rounded-md text-sm ring-blue-600 outline-none select-none focus-visible:ring-2 focus-visible:ring-offset-2"
    :class="[sizeClass, variantClasses[variant], disabled ? 'pointer-events-none opacity-50' : '']"
    :disabled
  >
    <component v-if="iconLeft" :is="iconLeft" class="size-4" :class="{ 'opacity-0': loading }" />
    <slot name="leading"></slot>
    <div v-if="$slots.default" class="inline" :class="{ 'opacity-0': loading }">
      <slot></slot>
    </div>
    <slot name="trailing"></slot>
    <div v-if="loading" class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 transform">
      <LoaderCircleIcon class="size-5 animate-spin" />
    </div>
  </component>
</template>
