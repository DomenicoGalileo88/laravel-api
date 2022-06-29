<template>
  <div>
    <WorkInProgress/>

    <section class="posts">
      <div class="container">
        <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-4">
          <div class="col" v-for="post in posts" :key="post.id">
            <div class="post card">
              <img :src="'storage/' + post.cover_image" :alt="post.title">
              <div class="card-body">
                <h3>{{post.title}}</h3>
                <p>{{post.content}}</p>
              </div>
              <div class="card-footer">
                <div class="row">
                  <div class="col">
                    <div class="author">
                      <strong>Author: </strong>
                      {{post.user.name}}
                    </div>
                  </div>

                  <div class="col">
                    <span v-if="post.category">
                      <strong>Category: </strong>
                      {{post.category.name}}
                    </span>

                    <div class="tags" v-if="post.tags.length > 0">
                      <strong>Tags: </strong>
                      <ul>
                        <li v-for="tag in post.tags" :key="tag.id">
                          {{tag.name}}
                        </li>
                      </ul>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import WorkInProgress from '../components/WorkInProgress';
export default {
  name: "App",
  components: {WorkInProgress},
  data(){
    return{
      posts: '',
    }
  },
  mounted(){
    console.log('mounted');
    axios.get('/api/posts').then(response => {
      console.log(response);
      this.posts = response.data.data;
    })
  }
}
</script>