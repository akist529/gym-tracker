<template>
    <div class="modal-bg">
        <slot></slot>
    </div>
</template>

<style>
.modal-bg {
    background-color: rgba(0, 0, 0, 0.8);
    width: 100vw;
    height: 100vh;
    position: fixed;
    top: 0;
    left: 0;
}
</style>
