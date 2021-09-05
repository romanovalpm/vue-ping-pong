<template>
    <div class="game-board">
        <GamePlatform position="top"/>
        <GameBall />
        <GamePlatform position="bottom"/>
    </div>
</template>

<script>
import GameBall from "@/components/GameBall";
import GamePlatform from "@/components/GamePlatform";

export default {
    name: 'GameBoard',
    components: {
        GameBall,
        GamePlatform,
    },
};
</script>

<style>
.game-board {
    width: 400px;
    height: 400px;
    background-color: #6A6E72;
    position: relative;
    overflow: hidden;
}
</style>
