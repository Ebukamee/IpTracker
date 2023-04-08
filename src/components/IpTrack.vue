<script>
export default {
  data() {
    return {
      message:1+1000000,
      Ip:"",
      Info:{},
      Address:'',
      location:'',
      time:'',
      isp:'',
      error:'',
      err:false
    };
  },
  methods: {
    async onFetch() {
      var api='https://geo.ipify.org/api/v2/country?apiKey=at_VQNjh8gAGCZrDciUIiRreWz5kgDHR&ipAddress=0.0.0.0';
      api=api.replace('0.0.0.0',this.Ip)
      try {
        const res = await fetch(api)
      .then((response) => response.json())
      .then((data) => this.info=data)
      .then(() => console.log(this.info))
      .then(()=> {
        this.Address=this.info.ip
        this.location=`${this.info.location.region},${this.info.location.country}`
        this.isp=this.info.isp
        this.time=`GMT ${this.info.location.timezone}`
      })
      }
    catch(err) {
      this.err=true
      this.error='Input correct IPv4 or IPv6 address.'
      setTimeout(() => {
        this.err=false
      }, 5000);
    }
   }
  },
  mounted() {
    // this.Info = this.onFetch.response.json()
  }, 
};
</script>

<template>
  <div id="main">
    <header>
      <h1>IP Address Tracker</h1>
      <div class="search-input">
        <input type='text' v-model='Ip'/>
        <button @click='onFetch' :disabled="this.Ip==''">
          <img src='../assets/images/icon-arrow.svg' alt='search' />
        </button>
      </div>
    </header>
    <main>
      <div class="main-container">
        <div class="contained">
          <p>IP ADDRESS</p>
          <h4>{{ Address }}</h4>
        </div>
        <div class="contained">
          <p>LOCATION</p>
          <h4>{{ location }}</h4>
        </div>
        <div class="contained">
          <p>TIMEZONE</p>
          <h4>{{ time }}</h4>
        </div>
        <div class="contained" id='none'>
          <p>ISP</p>
          <h4>{{ isp }}</h4>
        </div>
      </div>
      <div id="err" v-show="err">
        <p>{{ error }}</p>
      </div>
    </main>
  </div>
</template>

<!--style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style-->
