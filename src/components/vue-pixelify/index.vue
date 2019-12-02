<template>
<div>
    <canvas :ref="'canvas'"></canvas>
</div>
    
</template>

<script>
export default {
    name: 'vue-pixelify',
    mounted () {
        this.pixelify(
            this.src,
            this.width,
            this.height,
            this.pixelSize,
            this.centered,
            this.fillTransparencyColor,
            this.$refs.canvas
        );
    },
    beforeUpdate () {
        this.pixelify(
            this.src,
            this.width,
            this.height,
            this.pixelSize,
            this.centered,
            this.fillTransparencyColor,
            this.$refs.canvas
        );
    },
    methods: {
        pixelify(src, width, height, pixelSize, centered, fillTransparencyColor, el) {
            pixelSize = parseInt(pixelSize, 10);
            // create img that will be later painted into the canvas
            let img = new Image();
            img.src = src
            // once image is loaded..
            img.onload = () => {
                const canvas = el;
                const ctx = canvas.getContext("2d");
                img.width = width ? width : img.width;
                img.height = height ? height : img.height;
                
                canvas.width = img.width;
                canvas.height = img.height;
                ctx.drawImage(img, 0, 0, img.width, img.height);
                this.paintPixels(ctx, img, pixelSize, centered, fillTransparencyColor);
                img = null;
            };
        },
        paintPixels(ctx, img, pixelSize, centered, fillTransparencyColor) {
            if (!isNaN(pixelSize)) {
                for (let x = 0; x < img.width + pixelSize; x += pixelSize) {
                    for (let y = 0; y < img.height + pixelSize; y += pixelSize) {
                        let xColorPick = x;
                        let yColorPick = y;

                        if (x >= img.width) {
                            xColorPick = x - (pixelSize - (img.width % pixelSize) / 2) + 1;
                        }
                        if (y >= img.height) {
                            yColorPick = y - (pixelSize - (img.height % pixelSize) / 2) + 1;
                        }

                        const rgba = ctx.getImageData(xColorPick, yColorPick, 1, 1).data;
                        ctx.fillStyle =
                            rgba[3] === 0
                            ? fillTransparencyColor
                            : `rgba(${rgba[0]},${rgba[1]},${rgba[2]},${rgba[3]})`;

                        if (centered) {
                            ctx.fillRect(
                            parseInt(x - (pixelSize - (img.width % pixelSize) / 2), 10),
                            parseInt(y - (pixelSize - (img.height % pixelSize) / 2), 10),
                            pixelSize,
                            pixelSize
                            );
                        } else {
                            ctx.fillRect(x, y, pixelSize, pixelSize);
                        }
                    }
                }
            }
        },
    },
    props: {
        src: {
            type: String,
            required: true
        },
        pixelSize: {
            type: Number
        },
        width: {
            type: Number
        },
        height: {
            type: Number
        },
        centered: {
            type: Boolean,
            default: false
        },
        fillTransparencyColor: {
            type: String,
            default: 'white'
        }
    }
}
</script>