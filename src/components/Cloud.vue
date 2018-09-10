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
            type: [Function, Number],
        },
        font: {
            type: [String, Function],
            default: 'serif'
        },
        width: {
            type: Number,
            default: 960,
        },
        height: {
            type: Number,
            default: 500,
        },
        padding: {
            type: Number,
            default: 0,
        }
    },

    mounted() {

        const wordCounts = this.data.map(
            text => ({ ...text })
        );
         
        const layout = cloud()
        .size([this.width, this.height])
        .words(wordCounts)
        .padding(this.padding)
        .rotate(this.rotate)
        .font("Impact")
        .fontSize(this.fontSizeMapper)
        .on('end', this.end);

        layout.start();
    },

    methods: {
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
            .style('font-family', "Impact")
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
