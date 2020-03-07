<template>
  <div class="container">
    <div>
      <logo />
      <h1 class="title">
        show-articles-by-xml
      </h1>
      <h2 class="subtitle">
        {{ article[0].LawTitle[0] }}
      </h2>
      <div class="article">
        <div class="capters" v-for="(capter, index2) in SupplProvision" v-bind:key="index2">{{capter.ArticleTitle[0]}}
          <div v-for="(paragraph, index3) in capter.Paragraph[0].ParagraphSentence[0].Sentence" v-bind:key="index3">
            {{paragraph["_"]}}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import ps from 'xml2js'

import Logo from '~/components/Logo.vue'

export default {
  components: {
    Logo
  },
  asyncData ({ params }) {
    return axios.get('/data/410AC0000000114_20160401_426AC0000000115.xml')
      .then((res) => {
        console.log(res)
        let aticle
        ps.parseString(res.data, (message, xmlres) => {
          // store.commit('article', xmlres)
          aticle = xmlres.Law
        })
        console.log(aticle)
        console.log(aticle.LawBody[0].SupplProvision[0].Article)
        return { article: aticle.LawBody, SupplProvision: aticle.LawBody[0].SupplProvision[0].Article }
      })
  }
}
</script>

<style>
.container {
  margin: auto 20px;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
.article {
  text-align: left;
}
</style>
