<script>
import * as d3 from "d3";
import * as cloud from 'd3-cloud';

const fill = d3.scaleOrdinal(d3.schemeCategory10);

export default {
    data() {
        return {
            layout: {},
            chart: {},
        }
    },

    props: {
        data: {
            type: Array,
            required: true,
        },
        fontSizeMapper: {
            type: Function,
            required: true,
        },
        onWordClick: {
            type: Function,
            default: (word) => { console.log(word) },
        },
        rotate: {
            type: [Function, String, Number],
            default: 0,
        },
        font: {
            type: [String, Function],
            default: 'Serif'
        },
        width: {
            type: [Number, String],
            default: 960,
        },
        height: {
            type: [Number, String],
            default: 500,
        },
        padding: {
            type: [Number, String],
            default: 0,
        },
        spiral: {
            type: String,
            default: "archimedean",
        }
    },

    mounted() {
        this.createCanvas()
    },

    watch: {
        data() {
            this.createCanvas()
        },
        rotate() {
            this.createCanvas()
        },
        font() {
            this.createCanvas()
        },
        padding() {
            this.createCanvas()
        },
        spiral() {
            this.createCanvas()
        },
    },

    methods: {
        createCanvas: function() {
            const wordCounts = this.data.map(
                text => ({ ...text })
            );

            d3.select(this.$el).selectAll('*').remove();

            const layout = cloud()
            .size([this.width, this.height])
            .words(wordCounts)
            .padding(this.padding)
            .spiral(this.spiral)
            .rotate(this.rotate)
            .font(this.font)
            .fontSize(this.fontSizeMapper)
            .on('end', this.end);

            layout.start();
        },
        end: function(words) {
            let cloud = d3.select(this.$el)
            .append('svg')
            .attr('width', this.width)
            .attr('height', this.height)
            .append('g')
            .attr('transform', `translate(${this.width / 2},${this.height / 2})`)
            .selectAll('text')
            .data(words)
            .enter()
            .append('text')
            .style('font-family', d => d.font)
            .style('font-size', d => {
                return `${d.size}px`
            })
            .style('fill', (d, i) => fill(i))
            .attr('text-anchor', 'middle')
            .attr('transform',d => { 
                return `translate(${[d.x, d.y]})rotate(${d.rotate})`
            })
            .text(d => d.text)
            .on('click', d => this.onWordClick(d));
        },
    }
 }
</script>

<template>
    <div class="wordCloud" ref="wordCloud"></div>
</template>
