<template>
    <div class="container">
        <div class="row">
             <h1>A&S Articles</h1>
            <div class="col-md-8">
                <div class="form-group">
                    <label for="query">Filter by text</label>
                    <input type="text" name="query" id="query" v-model="query" class="form-control">
                </div>
                <select name="" id="" v-model="tagFilter" class="form-control">
                    <option value="All">All</option>
                    <option value="Humanities & Arts">Humanities & Arts</option>
                    <option value="Social Sciences">Social Sciences</option>
                    <option value="Interdisciplinary">Interdisciplinary</option>
                </select>
                <div class="checkbox">
                    <label for="">
                        <input id="1" type="checkbox" value="All" v-model="tagFilterArr">All
                    </label>
                    <label for="">
                        <input type="checkbox" value="Humanities & Arts" v-model="tagFilterArr">Humanities & Arts
                    </label>
                    <label for="">
                        <input type="checkbox" value="Social Sciences" v-model="tagFilterArr">Social Sciences
                    </label>
                    <label for="">
                        <input type="checkbox" value="Interdisciplinary" v-model="tagFilterArr">Interdisciplinary
                    </label>
                </div>
            </div>
        </div>
        <h2>Topics</h2>
            <!-- {{allTopicsComputed}} -->
            <ul>
               <li v-for="(topic, index) in allTopicsComputed" :key="index">
                   <!-- <label for="hi">
                        <input type="checkbox"  >{{topic}}
                    </label> -->
                    <label :for="topic"><input type="checkbox" :value="topic" :id="topic">{{topic}}</label>
                   </li>
            </ul>
        
        
        <!-- checkboxes return t/f not value 
            <div class="checkbox">
            <label for="">
                <input id="1" type="checkbox" value="All" v-model="tagFilter">All
            </label>
            <label for="">
                <input type="checkbox" value="Humanities & Arts" v-model="tagFilter">Humanities & Arts
            </label>
            <label for="">
                <input type="checkbox" value="Social Sciences" v-model="tagFilter">Social Sciences
            </label>
            <label for="">
                <input type="checkbox" value="Interdisciplinary" v-model="tagFilter">Interdisciplinary
            </label>
        </div>
         -->
<app-card 
            v-for="article in filteredArticles" 
            :article="article.node" 
            :key="article.sourcenid"
            @turn-to-red="backgroundToRed">
        </app-card>
    </div>
</template>
<script>
const url = "https://api.myjson.com/bins/hrnoz";

import card from "./card.vue";
import axios from "axios";
export default {
  data() {
    return {
      query: "",
      tagFilter: "All",
      tagFilterArr: [],
      articles: []
    };
  },
  mounted() {
    this.getPosts();
  },
  methods: {
    getPosts() {
      axios
        .get(url)
        .then(response => {
          this.articles = response.data.nodes;
        })
        .catch(error => {
          console.log(error);
        });
    },
    backgroundToRed() {
      return { backgroundColor: "red" };
    }
  },
  computed: {
    filteredArticles: function() {
      var vm = this;
      var query = vm.query;
      var topic = vm.tagFilter;
      var topics = vm.tagFilterArr;
      if (query === "" && topic === "All") {
        // performance just return whole list
        return vm.articles;
      } else {
        return vm.articles.filter(function(article) {
          return (
            article.node.title.toLowerCase().includes(vm.query.toLowerCase()) &&
            (article.node.field_topic.includes(vm.tagFilter) || topic === "All")
          );
          console.log(article);
        });
      }
      console.log("query");
    },

    allTopicsComputed: function() {
      //const articles = [];
      const allTopics = [];
      var vm = this;
      //   console.log(vm.articles);
      //   vm.articles.forEach(article => {
      //       return article.node.title;
      //   });

      vm.articles.forEach(article => {
        article.node.field_topic.split(",").forEach(topic => {
          if (!allTopics.includes(topic.trim())) {
            allTopics.push(topic.trim());
          }
        });
      });
      return allTopics;
    }
  },
  components: {
    appCard: card
  }
};
</script>
<style>
</style>
