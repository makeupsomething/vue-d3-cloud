<template>
  <div id="app">
    <cloud class="cloud" v-if="wordCount" :data="wordCount" :fontSizeMapper="fontSizeMapper" :rotate="rotate" :font="font" :padding="padding" :spiral="spiral" />
    <br />
    <textarea v-model="words" cols="100" rows="20" />
    <div class="container">
      <span>Words will have a random rotation between</span>
      <br />
      <label>Min Degree</label>
      <input v-model="min" type="number" />
      <label>Max Degree</label>
      <input v-model="max" type="number" />
      <br />
      <label>Padding</label>
      <input v-model="padding" type="number" />
      <br />
      <label>Font</label>
      <select v-model="font">
        <option>Serif</option>
        <option>Helvetica</option>
        <option>Arial</option>
        <option>Times</option>
        <option>Times New Roman</option>
        <option>Courier</option>
        <option>Impact</option>
        <option>Georgia</option>
      </select>
      <br />
      <label>Spiral Style</label>
      <select v-model="spiral">
        <option>archimedean</option>
        <option>rectangular</option>
      </select>
    </div>
  </div>
</template>

<script>
//import Cloud from 'vue-d3-cloud'
import Cloud from '../../src/components/Cloud'

export default {
  name: 'app',
    data() {
    return {
      min: 0,
      max: 65,
      padding: 0,
      words: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Maecenas sed cursus metus. Proin posuere, risus vestibulum malesuada consectetur, est justo vehicula eros, et hendrerit velit eros nec sem. Fusce lacinia ex et urna suscipit lobortis. Sed sollicitudin sodales felis, in tincidunt erat pretium eget. Integer at ligula rutrum, faucibus massa eget, porttitor sem. Ut non porttitor lorem. In luctus nec dui quis finibus. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Donec ultrices leo vitae vehicula vehicula. Cras porttitor, quam eu lobortis luctus, nunc justo suscipit sem, sit amet semper velit ante quis turpis. Vestibulum mattis sollicitudin ullamcorper. Maecenas sit amet nulla vitae nisl blandit dictum. Proin pharetra eget nisl pharetra venenatis. Quisque interdum ullamcorper neque tincidunt ultricies.",
      fontSizeMapper: word => Math.log2(word.value*5) * 10,
      font: "Helvetica",
      spiral: "archimedean",
    }
  },
  components: {
    Cloud,
  },
  computed: {
    rotate: function() {
      let newMin = this.min
      let newMax = this.max
      return word => Math.random() * (newMax - newMin) + newMin
    },
    wordCount: function() {
      if(!this.words)
        return []
      
      let occurences = this.words.split(' ').reduce((allNames, name) => { 
        if (name in allNames) {
          allNames[name]++;
        } else {
          allNames[name] = 1;
        }
        return allNames;
      }, {});
      let tmp = []
      for(var text in occurences) {
        let obj = {}
        obj.text = text;
        obj.value = occurences[text]
        tmp.push(obj)
      }
      return tmp
    }
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 60px;
}

textarea {
  display: block;
  margin-left: auto;
  margin-right: auto;
}

svg {
  display: block;
  margin-left: auto;
  margin-right: auto;
}

.container {
  display: flex;
  margin: auto;
  flex-direction: column;
  flex-wrap: wrap;
  align-content: center;
  max-width: 200px;
}
</style>