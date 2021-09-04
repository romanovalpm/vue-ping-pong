<template>
    <div class="game-platform"
         :class="platformClass"
         :style="platformStyles"
    />
</template>

<script>
const LEFT_ARROW = 37;
const RIGHT_ARROW = 39;
const ONE_SECOND = 60;

export default {
    name: 'GamePlatform',
    props: {
        position: {
            type: String,
            required: true,
            validator(value) {
                return ['top', 'bottom'].indexOf(value) !== -1;
            },
        },
    },
    data() {
        return {
            movingLeft: false,
            movingRight: false,
            moveUpdater: null,
            movePosition: 50,
        };
    },
    mounted() {
        document.addEventListener('keydown', this.handleKeyDown);
        document.addEventListener('keyup', this.stopMovingPlatform);
    },
    computed: {
        platformClass() {
            return 'game-platform--' + this.position;
        },
        platformStyles() {
            return {transform: "translateX(-" + this.movePosition + "%) translateY(-50%)"};
        },
    },
    methods: {
        handleKeyDown(event) {
            if (event.keyCode !== LEFT_ARROW && event.keyCode !== RIGHT_ARROW) {
                return;
            }

            if (event.keyCode === LEFT_ARROW) {
                this.movingLeft = true;
                this.movePlatform('left');
            } else {
                this.movingRight = true;
                this.movePosition++;
                this.movePlatform('right');
            }
        },
        movePlatform(direction) {
            if (direction === 'left') {
                this.movePosition > 0 ? this.movePosition-- : 0;
            } else if (direction === 'right') {
                this.movePosition++;
            }

            this.moveUpdater = setInterval(this.movePlatform(direction), ONE_SECOND);
        },
        stopMovingPlatform() {
            this.movingLeft = false;
            this.movingRight = false;
            clearInterval(this.moveUpdater);
        },
    },
};
</script>

<style>
.game-platform {
    width: 100px;
    height: 10px;
    background-color: #FFFFFF;
    position: absolute;
    left: 50%;
    transition: .1s all;
}

.game-platform--top {
    top: 5%;
}

.game-platform--bottom {
    top: 95%;
}
</style>
