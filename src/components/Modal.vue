<script setup>
const props = defineProps({
    show: Boolean
});
</script>

<template>
<Transition name="Modal">
    <div v-if="show" class="ModalWrapper">
        <div class="ModalInner p-3">
            <slot />
            <div class="text-center">
                <button @click="$emit('close')" class="mt-6 rounded-md bg-yellow-300 p-3"><b>閉じる</b></button>
            </div>
        </div>
    </div>
</Transition>
</template>
<style>
.ModalWrapper{
    position: fixed;
    z-index: 9999;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0,0,0,.7);
    display: flex;
    transition: opacity 0.3s ease;
    overflow: hidden;
}
.ModalInner{
    width: 90vw;
    max-width: 500px;
    max-height: 90vh;
    background-color: #fff;
    margin: auto;
    transition: all .5s ease-in-out;
    -webkit-transform: translateY(0);
    transform: translateY(0);
    overflow-y: scroll;
}
@media (min-width: 768px) {
    .ModalInner{
        overflow-y: auto;
    }
}
.Modal-enter-from {
  opacity: 0;
}
.Modal-leave-to {
  opacity: 0;
}
.Modal-enter-from .ModalInner,
.Modal-leave-to .ModalInner {
    -webkit-transform: translateY(-100%);
    transform: translateY(-100%);
}
</style>