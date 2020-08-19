<template>
    <div>
        <!-- <svg width="200" height="100">
            <rect
                x="0"
                y="0"
                width="200"
                height="100"
                stroke="red"
                stroke-width="3px"
                fill="white"
            />
            <text
                x="50%"
                y="50%"
                dominant-baseline="middle"
                text-anchor="middle"
            >
                TEXT
            </text>
        </svg> -->
        <div id="app"></div>
    </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import { SVG } from '@svgdotjs/svg.js'
import mock from './mock'
export default {
    name: 'App',
    components: {
        // HelloWorld
    },
    methods: {
        circle(draw, options) {
            const { w, h, x, color, text } = options
            const drawHeight = draw.height()
            const y = (drawHeight - h) / 2
            const nested = draw
                .nested()
                .addTo(draw)
                .size(w, h)
                .move(x, y)
            nested
                .circle(w)
                .addTo(nested)
                .attr({ fill: color })
            const t = nested.text(text)
            t.font({
                family: 'Helvetica',
                size: 14,
                anchor: 'middle'
            })
                .build(false)
                .addTo(nested)
                .fill('#fff')
                .attr({
                    x: '50%',
                    // y: '55%',
                    'dominant-baseline': 'middle'
                })
            return nested
        },
        button(draw, options) {
            const { x, y, w, h, color, text } = options
            const nested = draw
                .nested()
                .addTo(draw)
                .size(w, h)
                .move(x, y)
            const rect = nested.rect()
            const t = nested.plain(text)

            rect.size(w, h)
                .radius(4)
                .addTo(nested)
                .move(0, 0)
                .attr({
                    fill: '#fff',
                    stroke: color,
                    'stroke-linecap': 'round',
                    'stroke-dasharray': '5, 5'
                })
            // text.addTo(nested)
            t.font({
                family: 'Helvetica',
                size: 14,
                anchor: 'middle'
            })
                .build(false)
                .addTo(nested)
                .fill(color)
                .attr({
                    x: '50%',
                    y: '55%',
                    'dominant-baseline': 'middle'
                })
            nested
                .linkTo(link => {
                    link.to('http://svgdotjs.github.io/').target('_blank')
                })
                .click(() => {
                    console.log(111)
                })
                .mouseover(() => {
                    rect.attr({
                        stroke: 'rgba(252,183,98,0.21)',
                        fill: color
                    })
                    t.fill('#fff')
                })
                .mouseout(() => {
                    t.fill(color)
                    rect.attr({ stroke: color, fill: '#fff' })
                })
            return nested
        },
        line(draw, options, btn, p) {
            const { x, y, w } = options
            console.log('x, y, w: ', x, y, w)
            // console.log('btn: ', btn)
            const _nodeH = btn.height()
            const parent = {
                x: p.x(),
                y: p.y() + (p.width() / 2),
                w: p.width()
            }
            const node = {
                x: btn.x(),
                y: btn.y()
            }
            console.log(parent, _nodeH)
            const path = `M${parent.x + parent.w} ${parent.y} C${parent.x + parent.w + 15} ${parent.y} ${node.x - 15} ${node.y + (_nodeH / 2)} ${node.x} ${node.y + (_nodeH / 2)}`
            console.log('path: ', path);
            draw.path(path)
                .fill('none')
                .stroke({
                    color: '#fcb73d',
                    width: 1,
                    linecap: 'round',
                    linejoin: 'round'
                })
        }
    },
    mounted() {
        const drawHeight = 150
        const buttonHeight = 32
        var draw = SVG()
            .addTo('#app')
            .size(1000, drawHeight)

        mock.forEach(item => {
            const parent = this.circle(draw, {
                w: 54,
                h: 54,
                x: 0,
                y: 0,
                color: '#fcb73d',
                text: item.name
            })
            item.list.forEach((v, index) => {
                const x = index * 150 + 100
                const baseH = drawHeight / v.length
                v.forEach((b, i) => {
                    const index = i + 1
                    const y =
                        (baseH * index - buttonHeight) / 2 + (baseH * i) / 2
                    const btn = this.button(draw, {
                        w: 100,
                        h: buttonHeight,
                        x: x,
                        y: y,
                        color: '#fcb73d',
                        text: b.name
                    })
                    this.line(
                        draw,
                        {
                            x: 54,
                            y: drawHeight / 2,
                            w: 100
                        },
                        btn,
                        parent
                    )
                })
            })
        })
    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    margin-top: 60px;
    /* background: #ccc; */
}
* {
    margin: 0;
    padding: 0;
}
</style>
