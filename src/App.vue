<template>
  <div id="app">
    <b-text
      class="text-component"
      :style="topRowElement.builtInStyle"
      :id="topRowElement.id"
      :font-size="topRowElement.fontSize"
      :color="topRowElement.color"
      :background-color="topRowElement.backgroundColor"
      @choose:element="handleChooseElement"
      >{{ topRowElement.text }}</b-text
    >
    <br />
    <b-text
      v-for="e in bottomRowElements"
      :key="e.id"
      class="text-component"
      :style="e.builtInStyle"
      :id="e.id"
      :font-size="e.fontSize"
      :color="e.color"
      :background-color="e.backgroundColor"
      @choose:element="handleChooseElement"
      >{{ e.text }}</b-text
    >
    <hr>
    <div class="controls-container">
      <div class="top-row-controls">
        <div>
          <label for="bgcolor" class="label">Background Color</label>
          <input type="color" id="bgcolor" name="bgcolor" v-model="currentTextElement.backgroundColor" />
        </div>

        <div>
          <label for="color" class="label">Font Color</label>
          <input type="color" id="color" name="bgcolor" v-model="currentTextElement.color" />
        </div>

        <div>
          <label for="fontSize" class="label">Font Size</label>
          <select id="fontSize" v-model="currentTextElement.fontSize">
            <option v-for="n in fontSizes" :value="n" :key="n">{{ n }}</option>
          </select>
        </div>
      </div>
      <textarea v-model="currentTextElement.text" rows="5" cols="50" class="text-field"></textarea>
      <div class="bottom-row-controls">
        <button @click="displayJson = true">Display JSON</button>
        <button @click="logJSON">Log JSON</button>
      </div>
      <div v-if="displayJson">
        <pre>{{ JSON.stringify(json, null, 2) }}</pre>
      </div>
    </div>
  </div>
</template>

<script>

import BText from './components/BText.vue'

export default ({
  name: 'App',
  components: {
    BText,
  },
  data: () => ({
    textElements: [
      {
        id: 0,
        text: 'Hi',
        builtInStyle: 'padding-left:3px; padding-right: 3px; border-radius: 4px;',
        color: '#ffffff',
        backgroundColor: '#f8bbd0',
        fontSize: 32,
      },
      {
        id: 1,
        text: 'My lovely',
        builtInStyle: 'padding-left:3px; padding-right: 3px; border-radius: 4px',
        color: '#ffffff',
        backgroundColor: '#f8bbd0',
        fontSize: 32,
      },
      {
        id: 2,
        text: 'little',
        builtInStyle: 'padding-left:3px; padding-right: 3px; border-radius: 4px',
        color: '#880e4f',
        backgroundColor: '#ffffff',
        fontSize: 54,
      },
      {
        id: 3,
        text: 'Ponny',
        builtInStyle: 'padding-left:3px; padding-right: 3px; border-radius: 4px',
        color: '#ba68c8',
        backgroundColor: '#f8bb00',
         fontSize: 28,

      },
    ],
    currentTextElementId: 0,
    displayJson: false,
  }),
  computed: {
    fontSizes() {
      const sizes= []
      for (let i = 8; i <= 96; i++) sizes.push(i)
      return sizes
    },
    currentTextElement() {
      return this.textElements.find(e => e.id == this.currentTextElementId)
    },
    topRowElement() {
      return this.textElements[0]
    },
    bottomRowElements() {
      return this.textElements.slice(1)
    },

    json() {
      const textElements = this.textElements

      const { id, builtInStyle, ...firstElement } = textElements[0]
      const output = []

      output.push(firstElement)

      for (let i = 1; i < textElements.length; i++) {
        const prev = output[output.length - 1]
         const { id, builtInStyle, ...curr } = textElements[i]
        if (
          prev.fontSize === curr.fontSize &&
          prev.color === curr.color &&
          prev.backgroundColor === curr.backgroundColor
        ) {
          output[output.length - 1] = {
            text: `${prev.text} ${curr.text}`,
            color: prev.color,
            backgroundColor: prev.backgroundColor,
            fontSize: prev.fontSize,
          }
        } else {
          output.push(curr)
        }
      }

      return output
    },
  },
  methods: {
    handleChooseElement(id) {
      this.currentTextElementId = id
    },
    logJSON() {
      console.log(JSON.stringify(this.json, null, 2))
    },
  },
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

.text-component {
  color: rgb(255, 255, 255);
  opacity: 1;
  border-radius: 6px;
  transform: rotate(0deg);
  border: 0px solid rgb(70, 144, 247);
  padding: 10px;
  text-align: center;
  letter-spacing: 0px;
  font-size: 32px;
  font-family: Aleo;
  font-weight: 300;
  text-decoration: none;
  box-shadow: rgb(0, 0, 0) 0px 0px 0px 0px;
  text-transform: initial;
  font-style: normal;
  z-index: 2002;
  text-shadow: rgba(0, 0, 0, 0.2) 2px 2px 0px;
  line-height: 1.5em;
}

 .controls-container {
  width: 80%;
  margin: 2rem auto;
}

.top-row-controls {
  display: flex;
  margin: auto;
  justify-content: space-around;
}

.bottom-row-controls {
  width: 25%;
  margin: 1rem auto;
  display: flex;
  justify-content: space-between;
}

.label {
  margin-right: 10px;
}

.text-field {
  margin-top: 2rem;
}
</style>
