<template>
    <div class="header">
        <h1>2048</h1>
        <button>New Game</button>
        <p>score:{{score}}</p>
    </div>
</template>
<script>
    export default {
        name: 'Header2048',
        data() {
            return {
                score: 0
            }
        }
    };
</script>
<style>
.header {
    font-size: 25px;
    font-family: 'Arial';
}
button {
    font-size: 25px;
    color: #fff;
    background-color: #8f7a66;
    line-height: 30px;
    border-radius: 6px;
}
button:hover {
    background-color: #9f8b77;
}
</style>