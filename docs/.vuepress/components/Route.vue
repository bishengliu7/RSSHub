<template>
<div class="routeBlock" :id="path">
  <h3 class="name">{{name}} <Badge text="支持 BT" type="tip" v-if="supportBT"/> <Badge text="支持播客" type="tip" v-if="supportPodcast"/> <Author :uid=author /> <Badge text="反爬严格" type="warn" v-if="crawlerBadge"/>
    <a :href="'#'+path" aria-hidden="true" class="header-anchor">#</a>
  </h3>
  <p class="example">
    举例: <a :href="'https://rsshub.app'+ example " target="_blank">https://rsshub.app{{example}}</a>
  </p>
  <p class="path">
    路由: <code>{{ path }}</code>
  </p>
  <div v-if="path.match(/:.*?(\/|$)/g)">
  <p>
    参数:
  <ul><li class="params" v-for="(item, index) in path.match(/:.*?(\/|$)/g)">{{item.replace(':','').replace('/','').replace('?','')}}, {{(item.includes('?'))?'可选':'必选'}} - <span v-html="renderMarkdown(paramsDesc[index])"></span></li></ul> </p>
  </div>
  <div v-else><p>参数: 无</p></div>
  <slot></slot>
</div>
</template>
<script>
import Author from "./Author.vue"
export default {
  components:{
      'Author': Author
  },
  props: {
    author: {
      type: String,
      default: 'DIYgod'
    },
    name: {
      type: String,
      required: true
    },
    path: {
      type: String,
      required: true
    },
    example: {
      type: String,
      required: true
    },
    paramsDesc: {
      type: [Array, String],
      default: '无'
    },
    crawlerBadge: {
      type: String,
      default: null
    },
    supportBT: {
      type: String,
      default: null
    },
    supportPodcast: {
      type: String,
      default: null
    }
  },
  methods: {
    renderMarkdown(item) {
    const md = require('markdown-it')({
            html: true,
    });
        return md.render(item);
    },
  }
}
</script>
<style>
li.params p {
  display: inline;
  }
.routeBlock {
  margin: 1rem 0 2rem;
}
#app .page .badge.tip {
  background-color: #FFB74D;
}
</style>
