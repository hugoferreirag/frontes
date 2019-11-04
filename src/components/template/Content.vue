<template>
    <div class="content">
        <router-view></router-view>
    </div>
</template>

<script>
export default {
    name: 'Content'
}
</script>

<style>
    .content {
        grid-area: content;
        background-image: url("https://www.google.com/url?sa=i&source=images&cd=&ved=2ahUKEwiIgq_km9HlAhW3IrkGHWPdBnoQjRx6BAgBEAQ&url=https%3A%2F%2Frobertveritas.com%2F2017%2F12%2F26%2Fblockchain-is-changing-the-world-right-now%2F&psig=AOvVaw1s2e-21yWkPuYNoBd8yi-U&ust=1572979591119553");
        padding: 20px;
    }
</style>
