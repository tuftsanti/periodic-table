<template>
<div id="element">
  <section>
    <h1> The Periodic Table of Elements </h1>
    <div v-if="elements.length === 0" class="loading">Loading...</div>
    <div v-for="element in elements" class="element-contain">
      <div class="element-img">
        <img :src="element.img" height="350" />
      </div>
      <div class="element-info">
        <h2>{{ element.name }}</h2>
        <p class="bright">{{ element.tagline }}</p>
        <p><span class="bright">Description:</span> {{ element.desc }}</p>
        <p><span class="bright">Tips:</span> {{ element.tips }}</p>
        <h3 class="bright">Food Pairings</h3>
        <ul>
          <li v-for="item in element.food">
            {{ item }}
          </li>
        </ul>
      </div>
    </div>
  </section>
</div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      bottom: false,
      elements: []
    }
  },
  watch: {
    bottom(bottom) {
      if (bottom) {
        this.addelement()
      }
    }
  },
  created() {
    window.addEventListener('scroll', () => {
      this.bottom = this.bottomVisible()
    })
    this.addelement()
  },
  methods: {
    bottomVisible() {
      const scrollY = window.scrollY
      const visible = document.documentElement.clientHeight
      const pageHeight = document.documentElement.scrollHeight
      const bottomOfPage = visible + scrollY >= pageHeight
      return bottomOfPage || pageHeight < visible
    },
    addelement() {
      axios.get('https://neelpatel05.pythonanywhere.com/')
        .then(response => {
          console.log(response.data)
          let api = response.data[0];
          let apiInfo = {
            name : api.name,
            desc : api.formulae,
            img : api.image_url,
            tips : api.brewers_tips,
            tagline : api.tagline,
            food : api.food_pairing
          };
          this.elements.push(apiInfo)
          if (this.bottomVisible()) {
            this.addelement()
          }
      })
    }
  }
}
</script>

<style>
body {
  font-family: 'Archivo Narrow', sans-serif;
  background: #25859a;
}

h1, h2, h3, h4 {
  font-family: 'Fjalla One', sans-serif;
}

h1 {
  margin-top: 40px;
  color: white;
  text-align: center;
}

.loading {
  color: white;
  text-align: center;
  font-size: 20px;
}

.display, #app, .element-contain, .element-img {
  display: -webkit-box;
  display: flex;
  -webkit-box-pack: center;
          justify-content: center;
  align-content: center;
}

#app {
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
          flex-direction: column;
}

.element-contain {
  width: 50%;
  margin: 20px 24%;
  box-shadow: 0 2px 3px 1px rgba(30, 0, 0, 0.1);
}

.element-img {
  padding: 30px;
  background: #FF6542;
  border: 1px solid #88498F;
  border-right: 1px solid #f44822;
}

.element-info {
  background: #564154;
  color: white;
  padding: 30px;
  border: 1px solid #88498F;
}
.element-info .bright {
  color: #fcd7cf;
  font-family: 'Contrail One', sans-serif;
}

h3 {
  margin-bottom: 5px;
}

ul {
  margin-top: 5px;
}

</style>
