<template>
    <div class="media"  :class="{active: makeBlue}">
        <div class="media-left media-top">
                <img class="media-object" :src="article.field_image" alt="">
        </div>
        <div class="media-body">
            <h2 class="media-heading">{{article.title}}</h2>
            <h3>{{article.field_card_label}}</h3>
            <p>{{article.field_byline}} | {{article.field_dateline}}</p>
            <p>{{article.summary}}</p>
            <p>Dept/Pro: {{article.field_related_dept_prog}}</p>
            <p>Tags: {{article.field_topic}}</p>
            <p>tags[] {{tags}}</p>
            <a :href="fullUrl">Read moreyoyoyoyooy</a>
        </div>
    </div>
</template>

<script>

export default {
  props: ['article'],
  data: function(){
      return {
          topics: [],
          makeBlue: true
      }
  },
  methods: {
      turnBlue(){
          return {backgroundColor: 'red'};
      }
  },
  computed: {
      fullUrl(){
          // remove trailing (/)
          return this.article.path.slice(0, -1) + this.article.Path;
      },
      tags(){
          return this.article.field_topic.split(',');
      }
  },
  mounted(){
    this.makeBlue = false;
    this.$emit('turn-to-red');
    console.log(this.makeBlue);

  }
}
</script>

<style>
.media{
    margin-bottom: 3em;
}
.media-left{
    padding-right: 2em;
}
.media-object{
    max-width: 200px;
}
</style>
