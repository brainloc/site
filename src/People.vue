.<template>
  <div class="inner cover">
    <div class="container">
      <div class="row">
        <div class="col-lg-4 col-md-6" v-for="member in members">
          <div class="card people">
            <img class="card-img-top rounded-circle" v-bind:src="member.avatar_url" height="250" width="250">
            <div class="card-block">
              <h4 class="card-title">{{ member.login }}</h4>
              <a v-bind:href="member.html_url"><i class="fa fa-github fa-2x" aria-hidden="true"></i></a>
            </div>
          </div>
        </div>
      </div>
        <div class="loading" v-if="loading">
          <strong>Gathering folks...</strong>
        </div>

        <div v-if="error" class="error">
          <h4>Something went wrong</h4>
          <p class="text-muted">{{ error }}</p>
        </div>
      </div>
    </div>
</template>

<script>
export default {
  data () {
    return {
      loading: false,
      members: null,
      error: null
    }
  },
  created () {
    this.fetchMembers();
  },
  watch: {
    '$route': 'fetchMembers'
  },
  methods: {
    fetchMembers () {
      this.error = this.members = null;
      this.loading = true;
      this.$http.get('https://api.github.com/orgs/inexorgame/members').then((response) => {
        this.loading = false;
        response.json();

        this.members = response.body;
      }, (response) => {
        this.loading = false;
        this.error = response.statusText;
      });
    }
  }
}
</script>

<style scoped>
.people {
  background-color: rgba(0,0,0, 0.5);
  border-color: transparent;
  border-radius: 100px 100px 20px 20px;
  margin: 50px;
}

.people h4:first-letter {
  text-transform:none;
}

.people {
  padding: 10px;
  width: 12rem;
  height: 20rem;
  margin: 1rem auto;
}
.people img {
  width: 100%;
  height: auto;
}

.people a {
  color:white;
  position: absolute;
  bottom: 1rem;
  left: 10%;
  right: 10%;
}
.people a:hover {
  color:white;
}

.people a {
  color:white;
}
.people a:hover {
  color:white;
  text-shadow:1px 1px 4px black;
}

.cardblock h4 {
  text-align: center;
}

.card-title {
  margin-top:20px;
}
</style>
