<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
import jwt_decode from "jwt-decode";
import { constants } from "crypto";
export default {
  name: "app",
  components: {},
  created() {
    if (localStorage.token) {
      const decoded = jwt_decode(localStorage.token);
      // token存储在vuex中 decoded是空的话，代表没授权，有内容的话 有授权
      this.$store.dispatch("setAuthenticated", !this.isEmpty(decoded));
      this.$store.dispatch("setUser", decoded);
    }
  },

  methods: {
    isEmpty(value) {
      return (
        value === undefind ||
        value === null ||
        (typeof value === "object" && Object.keys(value).length === 0) ||
        (typeof value === "string" && value.trim().length === 0)
      );
    }
  }
};
</script>
<style>
html,
body,
#app {
  width: 100%;
  height: 100;
}
</style>
