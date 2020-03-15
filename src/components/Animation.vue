<template>
    <div id="Component_Animation" :style="'width: '+configWork.width+'px'">
        <div id="canvas-wrapper" :style="'width: '+configWork.width+'px;height:'+configWork.height+'px;'">
            <canvas id="canvas-paper"
                    :width="configWork.width"
                    :height="configWork.height"
                    ref="canvas" hidpi="off"></canvas>
        </div>
        <div id="toolbar">
            <div class="columns has-text-centered">
                <div class="column has-text-right"></div>
                <div class="column has-text-centered">
                    <b-button type="is-dark" @click="play()" v-if="(!isPlaying)"> ></b-button>
                    <b-button type="is-dark" @click="pause()" v-else> ||</b-button>
                </div>
                <div class="column"></div>
            </div>
        </div>
    </div>
</template>

<script>

    import paper from 'paper';
    // import view from 'paper';

    export default {
        name: "Animation"
        , props: []
        , components: {}
        , mounted() {
            this.init();
        }
        , data() {
            return {
                pCanvas: null
                , pView: null
                , radius: 150
                , titleWork: "title of your work"
                , configWork: {
                    width: 1000
                    , height: 600
                }
                , circle: null
                , scale: 1
                , isPlaying: true
                , type: 'grow'
            }
        }
        , watch: {}
        , methods: {
            init() {
                paper.install(window);
                paper.setup(this.$refs.canvas);
                this.pCanvas = new paper.Project(this.$refs.canvas);
                this.pView = paper.view;
                this.renderSetup();
                this.pView.onFrame = this.onFrameHandler;
            }
            , renderSetup() {
                // --------------------------------------------------------------------------------- render a background
                let _start = new paper.Point(0, 0);
                let _end = new paper.Point(this.configWork.width, this.configWork.height);
                let _background = new paper.Path.Rectangle(_start, _end);
                _background.fillColor = new paper.Color(0.9, 0.9, 0.9);
                // --------------------------------------------------------------------------------------- render circle
                let center = new paper.Point(this.configWork.width / 2, this.configWork.height / 2);
                let color = paper.Color.random();
                this.circle = new paper.Path.Circle(center, 5);
                this.circle.fillColor = color;
                this.circle.scale(this.scale);
                // ------------------------------------------------------------------------------------ titling the work
                let text = new paper.PointText(new paper.Point(this.configWork.width / 2, this.configWork.height - 30));
                text.justification = 'center';
                text.fillColor = 'grey';
                text.fontFamily = 'serif';
                text.fontSize = 20;
                text.content = '“' + this.titleWork + '”';
            }
            , onFrameHandler() {
                if (this.isPlaying) {
                    this.renderFrame();
                }
            }
            , renderFrame() {
                if (this.type == 'grow') {
                    this.scale += 0.0001;
                    this.circle.scale(this.scale);
                } else {
                    this.circle.fillColor.hue += 1;
                    this.scale -= 0.0001;
                    this.circle.scale(this.scale);
                }
                if (this.scale >= 1.02) this.type = 'shrink';
                if (this.scale <= 0.98) this.type = 'grow';
            }
            , play() {
                this.isPlaying = true;
            }
            , pause() {
                this.isPlaying = false;
            }
        }
        , computed: {}
    }
</script>

<style scoped>

    #Component_Animation {
        margin-left: auto;
        margin-right: auto;
        padding-top: 2em;
    }

    #canvas-wrapper {
        margin: 0;
        padding: 0;
        display: block;
        background-color: #efefef;
    }

    #toolbar {
        padding-top: 2em;
    }

</style>
