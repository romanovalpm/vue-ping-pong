<template>
    <div class="game-ball"></div>
</template>

<script>
export default {
    name: 'GameBall',
};
</script>

<style>
.game-ball {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    background-color: #FFFFFF;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translateX(-50%) translateY(-50%);
}
</style>
