<template>
    <div class="star" :class="starType">
        <span v-for="itenClass in itenClasses" :class="itenClass" class="star-item" track-by="$index"></span>
    </div>
</template>
<script>
    const LENGTH = 5;
    const CLS_ON = 'on';
    const CLS_HALF = "half";
    const CLS_OFF = "off"
    export default {
        props: {
            size: {
                type: Number
            },
            score: {
                type: Number
            }
        },
        computed: {
            starType () {
                return 'star-' + this.size
            },
            itenClasses() {
                let result = [];
                let score = Math.floor(this.score *2)/2;//向下取0.5的值
                let hasDecimal = score % 1 !== 0;//判断有0.5的话取半
                let integer = Math.floor(score);//整数取整个星
                for(let i = 0; i<integer; i++) {
                    result.push(CLS_ON);//循环整数的给CLS_ON
                }
                if(hasDecimal) {
                    result.push(CLS_HALF); //判断有0.5的给CLS_HALF
                }
                while (result.length < LENGTH) {
                    result.push(CLS_OFF); // 剩下的给CLS_OFF
                }
                return result;
            }
        }
    }
</script>
<style lang="stylus" rel="stylesheet/stylus">
@import "../../assets/stylus/mixin"
    .star
        .star-item
            display: inline-block
            background-repeat: no-repeat
        &.star-48
            .star-item
                width: 20px
                height: 20px
                margin-right: 22px
                background-size: 20px 20px
                &:last-child
                    margin-right: 0
                &.on
                    background-image: url("star48_on@2x.png")
                    @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3)
                    	background-image:  url("star48_on@3x.png")
                &.half
                    background-image: url("star48_half@2x.png")
                    @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3)
                    	background-image:  url("star48_half@3x.png")
                &.off
                    background-image: url("star48_off@2x.png")
                    @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3)
                    	background-image:  url("star48_off@3x.png")
        &.star-36
            .star-item
                width: 15px
                height: 15px
                margin-right: 6px
                background-size: 15px 15px
                &:last-child
                    margin-right: 0
                &.on
                   background-image: url("star36_on@2x.png")
                   @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3)
                    	background-image:  url("star36_on@3x.png")
                &.half
                	background-image: url("star36_half@2x.png")
                	@media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3)
                    	background-image:  url("star36_half@3x.png")
                &.off
                	background-image: url("star36_off@2x.png")
                	@media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3)
                    	background-image:  url("star36_off@3x.png")
        &.star-24
            .star-item
                width: 10px
                height: 10px
                margin-right: 3px
                background-size: 10px 10px
                &:last-child
                    margin-right: 0
                &.on
                    background-image: url("star24_on@2x.png")
                    @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3)
                    	background-image:  url("star24_on@3x.png")
                &.half
                    background-image: url("star24_half@2x.png")
                    @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3)
                    	background-image:  url("star24_half@3x.png")
                &.off
                    background-image: url("star24_off@2x.png")
                    @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3)
                    	background-image:  url("star24_off@3x.png")
</style>