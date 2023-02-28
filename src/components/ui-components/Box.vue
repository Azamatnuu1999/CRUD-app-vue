<template>
    <div class="box">
        <slot></slot>
    </div>
</template>

<script>
export default {
    name: 'Box'
}
</script>

<style>
    .box{
        padding: 1.5rem;
        margin-top: 1.5rem;
        border-radius: 4px;
        background-color: #fcfaf5;
        box-shadow: 10px 10px 10px rgba(0, 0, 0, 0.15);
    }
</style>