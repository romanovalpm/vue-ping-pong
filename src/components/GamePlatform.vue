<template>
    <div class="game-platform"
         :class="platformClass"
         :style="platformStyles"
    />
</template>

<script>
const LEFT_ARROW = 37;
const RIGHT_ARROW = 39;
const KEY_A = 65;
const KEY_D = 68;
const UPDATE_INTERVAL = 10;
const MIN_POSITION = 50; //px
const MAX_POSITION = 350; //px
const MOVE_STEP = 5; //px

export default {
    name: 'GamePlatform',
    props: {
        position: {
            type: String,
            required: true,
            validator(value) {
                // eslint-disable-next-line
                return ['top', 'bottom'].indexOf(value) !== -1;
            },
        },
    },
    data() {
        return {
            moveDirection: null,
            moveUpdater: null,
            movePosition: 200,
        };
    },
    mounted() {
        document.addEventListener('keydown', this.handleKeyDown);
        document.addEventListener('keyup', this.handleKeyUp);
    },
    computed: {
        platformClass() {
            return 'game-platform--' + this.position;
        },
        platformStyles() {
            return {left: this.movePosition + "px"};
        },
        leftKey() {
            return this.position === 'top' ? KEY_A : LEFT_ARROW;
        },
        rightKey() {
            return this.position === 'top' ? KEY_D : RIGHT_ARROW;
        },
    },
    methods: {
        handleKeyDown(event) {
            if (event.keyCode !== this.leftKey && event.keyCode !== this.rightKey) {
                return;
            }

            if (event.keyCode === this.leftKey) {
                this.moveDirection = 'left';
                this.movePlatform();
            } else {
                this.moveDirection = 'right';
                this.movePlatform();
            }
        },
        movePlatform() {
            if (!this.moveDirection) return;

            if (this.moveDirection === 'left') {
                this.movePosition > MIN_POSITION ? this.movePosition -= MOVE_STEP : MIN_POSITION;
            } else if (this.moveDirection === 'right') {
                this.movePosition < MAX_POSITION ? this.movePosition += MOVE_STEP : MAX_POSITION;
            }

            if (!this.moveUpdater) {
                this.moveUpdater = setInterval(this.movePlatform, UPDATE_INTERVAL);
            }
        },
        handleKeyUp(event) {
            if (event.keyCode !== this.leftKey && event.keyCode !== this.rightKey) {
                return;
            }

            this.moveDirection = null;
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
    transform: translateX(-50%) translateY(-50%);
    transition: .1s all;
}

.game-platform--top {
    top: 5%;
}

.game-platform--bottom {
    top: 95%;
}
</style>
