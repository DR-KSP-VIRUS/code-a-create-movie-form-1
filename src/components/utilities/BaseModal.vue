<template>
    <div
        :class="isOpen ? ' fixed z-10 top-0 left-0 bottom-0 right-0 bg-black/30 flex place-items-center place-content-center min-w-full px-4 backdrop-blur-sm' : 'hidden'">
        <div class="relative mt-4 bg-white w-full rounded md:max-w-[640px]">
            <slot name="heading"></slot>
            <slot name="form"></slot>
        </div>
    </div>
</template>

<script setup>

const props = defineProps({
    isOpen: {
        type: Boolean,
        required: true
    }
});

</script>