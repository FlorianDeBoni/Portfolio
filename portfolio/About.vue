<template>
    <div style="display: flex; align-items: center; flex-direction: column;">
        <canvas 
            id="myCanvas" 
            width="300"
            height="300"
            @mousedown="beginDrawing"
            @mouseup="stopDrawing"
            @mousemove="keepDrawing"
            @mouseleave="stopDrawing">
        </canvas>
        <button
            style="margin-top: 10px;"
            @click="clear"
        >Clear</button>
        <button
            style="margin-top: 10px;"
            @click="predict"
        >Predict</button>
    </div>
</template>

<script>
    // import * as tf from '@tensorflow/tfjs';

    // const model = await tf.loadLayersModel();
    // console.log(model);

    export default {
        data() {
            return {
                canvas: null,
                x: 0,
                y: 0,
                isDrawing: false
            }
        },
        methods: {
            drawLine(x1, y1, x2, y2) {
                let ctx = this.canvas;
                ctx.beginPath();
                ctx.strokeStyle = 'black';
                ctx.lineWidth = 3;
                ctx.moveTo(x1, y1);
                ctx.lineTo(x2, y2);
                ctx.stroke();
                ctx.closePath();
            },
            beginDrawing(e) {
                this.x = e.offsetX;
                this.y = e.offsetY;
                this.isDrawing = true;
            },
            keepDrawing(e) {
                if (this.isDrawing === true) {
                    this.drawLine(this.x, this.y, e.offsetX, e.offsetY);
                    this.x = e.offsetX;
                    this.y = e.offsetY;
                }
            },
            stopDrawing(e) {
                if (this.isDrawing === true) {
                    this.drawLine(this.x, this.y, e.offsetX, e.offsetY);
                    this.x = 0;
                    this.y = 0;
                    this.isDrawing = false;
                }
            },
            clear() {
                const ctx = this.canvas;
                ctx.clearRect(0, 0, ctx.canvas.width, ctx.canvas.height);
            },
            predict() {
                const c = document.getElementById("myCanvas");
                console.log(c.toDataURL('image/png'));
            }
        },
        mounted() {
            const c = document.getElementById("myCanvas");
            this.canvas = c.getContext('2d');
        }
    }
</script>

<style scoped>
    #myCanvas {
        border: 1px solid black;
    }
</style>
