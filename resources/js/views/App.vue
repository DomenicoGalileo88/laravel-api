<template>
  <div>
    <WorkInProgress/>

    <section class="posts">
      <div class="container">
        <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-4">
          <div class="col" v-for="post in posts.data" :key="post.id">
            <div class="post card">
              <img :src="'storage/' + post.cover_image" :alt="post.title">
              <div class="card-body">
                <h3>{{post.title}}</h3>
                <p>{{post.content}}</p>
              </div>
              <div class="card-footer">
                <div class="row">
                  <div class="col">
                    <div class="author" v-if="post.user">
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

        <nav aria-label="Page navigation">
          <ul class="pagination justify-content-center pt-5">
            <li class="page-item" v-if="posts.current_page > 1">
              <a class="page-link" href="#" aria-label="Previous" @click.prevent='getAllPosts(posts.current_page - 1)'>
                <span aria-hidden="true">&laquo;</span>
                <span class="visually-hidden">Previous</span>
              </a>
            </li>
            <li :class="{'page-item' : true, 'active' : page == posts.current_page}" v-for="page in posts.last_page" :key="page">
              <a class="page-link" href="#" @click='getAllPosts(page)'>{{page}}</a></li>
            
            <li class="page-item" v-if="posts.current_page < posts.last_page">
              <a class="page-link" href="#" aria-label="Next" @click.prevent='getAllPosts(posts.current_page + 1)'>
                <span aria-hidden="true">&raquo;</span>
                <span class="visually-hidden">Next</span>
              </a>
            </li>
          </ul>
        </nav>
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
  methods: {
    getAllPosts(postPage){
      axios.get('/api/posts', {
        params: {
          page: postPage,
        }
      }).then(response => {
      console.log(response);
      this.posts = response.data;
    })
    }
  },
  mounted(){
    console.log('mounted');
    this.getAllPosts(1);
  },
}
</script>