<script>
export default {
  name: "ElestioStaticPage",
  data() {
    return {
      host: "?",
      ip: "?",
      location: "?",
      latency: "? ms",
    };
  },
  mounted() {
    this.host = window.location.host;
    this.getVariablesInfo();
    this.getlatency();
  },
  methods: {
    async getVariablesInfo() {
      let visitorInfos = null;
      if (localStorage.getItem("visitorInfos") == null) {
        await fetch("https://ipinfo.io/json")
          .then(async function (response) {
            visitorInfos = await response.json();
            localStorage.setItem("visitorInfos", JSON.stringify(visitorInfos));
          })
          .catch(function (error) {

          });
      } else {
        visitorInfos = JSON.parse(localStorage.getItem("visitorInfos"));
      }

      this.ip = visitorInfos.ip ? visitorInfos.ip : "?";
      this.location = visitorInfos.country
        ? visitorInfos.country + ", " + visitorInfos.city
        : "?";
    },
    getlatency() {
      setInterval(async () => {
        const started = new Date().getTime();
        const url = "/data.json?t=" + +new Date();
        await fetch(url)
          .then(function (response) {
            const ended = new Date().getTime();
            const milliseconds = ended - started;
            document.getElementById("latency").innerHTML = milliseconds + " ms";
          })
          .catch(function (error) {
            document.getElementById("latency").innerHTML = "? ms";

          });
      }, 1000);
    },
  },

}
</script>


<template>
  <div>

    <img src="../static/elestio-logo.svg" alt="elestio logo" class="elestio-logo" />

    <div class="app-body">
      <div class="app-heading" style="margin-bottom: 40px;">
        <h1>Welcome to Elestio</h1>
        <h4>Deploy your apps quickly with the easiest CI/CD system</h4>
      </div>

      <p class="app-info-block">
        This Host <strong id="host" class="subVal">{{ host }}</strong>
      </p>

      <div v-if="ip !== '?'">
        <p class="app-info-block">
          Your IP <strong id="yourIP" class="subVal">{{ ip }}</strong>
        </p>
      </div>

      <div v-if="location !== '?'">
        <p class="app-info-block">
          Your Location<strong id="yourLocation" class="subVal">{{ location }}</strong>
        </p>
      </div>

      <p class="app-info-block">
        Latency to server <strong id="latency" class="subVal">{{ latency }}</strong>
      </p>

      <div class="app-deploy">

        <a href="https://dash.elest.io/deploy?source=cicd&social=Github&url=https://github.com/elestio-examples/static"
          class="btn mb-10-m btn-try">Deploy on Elestio
        </a>

      </div>
    </div>

    <div class="area">
      <ul class="circles">
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
      </ul>
    </div>
  </div>

</template>



<style>
@import "../assests/elestio.css";
</style>