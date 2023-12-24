<template>
  <div class="wrapper">
    
    <section>
      <h1>Search</h1>
      <p>Find subscribers by name, email adderess or Id.</p>
      <div class="search__wrapper">
        <input id="search" placeholder="Search"  @keyup.enter="query(true)" /> <button class="button" @click="query(true)"><span class="material-symbols-outlined">search</span></button>
      </div>
      <div id="warn" class="message" v-if="warn">Enter at least 3 characters</div> 
      <div id="loading" class="message" v-if="loading">Loading...</div> 
      <div id="noresults" class="message" v-if="noresults">No results. Try a different search.</div> 
    </section>
    <section>
      <card :subscribers="subscribers" />
      <div class="pagination" v-if="subscribers.length > 0">
        Page {{currentPage+1}} of {{totalPages}}
        <div class="pageination__controls">
          <span :class="this.currentPage + 1 === 1 ? 'control material-symbols-outlined disabled' : 'control material-symbols-outlined'" @click="pagePrevious()">chevron_left</span>
          <span id="next" :class="this.currentPage + 1 === this.totalPages ? 'control material-symbols-outlined disabled' : 'control material-symbols-outlined'" @click="pageNext()">chevron_right</span>
        </div>
      </div>
    </section>
  </div>
</template> 
<script scoped>
export default {
 data() {
    return {
      subscribers: [],
      currentPage: 0,
      totalPages: 0,
      warn: false,
      loading: false,
      noresults: false,
      search:''
    }
  },
  methods: {
    async query(page){
      this.search = document.getElementById('search').value
      if(this.search.length < 3 && this.search.length != 0) {
        this.warn = true
        this.subscribers = []
      } else {
        //reset results
        if(page)this.currentPage = 0
        //push query params to url
        this.$router.push(`/searchsubscribers?pageIndex=${this.currentPage}&search=${this.search}`)
        this.warn = false
        this.loading = true
        await fetch(`https://tech-test.questline.com/searchsubscribers?pageIndex=${this.currentPage}&search=${this.search}`)
          .then(res=>res.json())
          .then(json=>{
            this.subscribers = json.subscribers, 
            this.totalPages = json.totalResults % 10 > 0 ? (json.totalResults - (json.totalResults % 10)) / 10 + 1 : json.totalResults / 10,
            this.subscribers.length < 1 ? this.noresults = true : this.noresults = false,
            this.loading = false
            }
          )
      } 
    },
    pageNext(){
      if(this.currentPage + 1 <= this.totalPages){
        this.currentPage++
        this.query()
      }
    },
    pagePrevious(){
      if(this.currentPage + 1 > 1){
        this.currentPage--
        this.query()
      }
    }
  },
  mounted(){
    if(this.$route.query.search && this.$route.query.pageIndex ){
      this.loading = true
      fetch(`https://tech-test.questline.com/searchsubscribers?pageIndex=${parseInt(this.$route.query.pageIndex)}&search=${this.$route.query.search}`)
        .then(res=>res.json())
        .then(json=>{
          this.subscribers = json.subscribers, 
          this.totalPages = json.totalResults % 10 > 0 ? (json.totalResults - (json.totalResults % 10)) / 10 + 1 : json.totalResults / 10,
          this.subscribers.length < 1 ? this.noresults = true : this.noresults = false,
          this.loading = false,
          this.currentPage = parseInt(this.$route.query.pageIndex)
          }
        )
    } 
    else if (this.$route.query.search === ""){
      this.loading = true
      this.currentPage = this.$route.query.pageIndex 
      fetch(`https://tech-test.questline.com/searchsubscribers?pageIndex=${parseInt(this.$route.query.pageIndex)}&search=${this.$route.query.search}`)
        .then(res=>res.json())
        .then(json=>{
          this.subscribers = json.subscribers, 
          this.totalPages = json.totalResults % 10 > 0 ? (json.totalResults - (json.totalResults % 10)) / 10 + 1 : json.totalResults / 10,
          this.subscribers.length < 1 ? this.noresults = true : this.noresults = false
          }
        )
        .then(
          this.loading = false,
          this.currentPage = parseInt(this.$route.query.pageIndex)
          )
    }
  }
}
</script>
<style scoped lang="scss">
input {
  background-color: $gray-light;
  border: 0px;
  border-radius: $radius;
  color: $gray-dark;
  font-size: 1rem;
  padding: 16px 24px; 
  width: clamp(300px, 100%,  500px);
  &:focus {
    outline: none;
  }
}
.button {
  align-items: center;
  background-color: $dark;
  color: $white;
  border: none;
  border-radius: $radius;
  cursor: pointer;
  display: inline-flex;
  font-weight: 700;
  justify-content: center;
  line-height: 0;
  padding: 12px 14px;
  transition: background-color 0.5s;
  .material-symbols-outlined {
    font-size: 1.6rem;
  }
  &:hover {
    background-color: #555;
  }
}
.search__wrapper {
  display: flex;
  gap: 15px;
  justify-content: center;
}
.message {
  color: $gray-dark;
  font-size: 14px;
  line-height: 18px;
  padding-top: 10px;
}
#warn {
  color: #D21212;
}
.pagination {
  font-size: 14px;
  font-weight: 700;
}
.pageination__controls {
  align-items: center;
  display: flex;
  gap: 10px;
  justify-content: center;
  padding: 10px 0;
}
.control {
  color: $white;
  background-color: $dark;
  border-radius: $radius;
  padding: 8px 6px;
  transition: background-color 0.5s;
  user-select: none;
  cursor: pointer;
  &:hover {
    background-color: $highlight;
  }
}
.disabled {
  background-color: #888;
  cursor: not-allowed;
}
</style>