<template>
  <div id="app">
    <v-app id="inspire" :dark="dark">
      <v-toolbar fixed app>
        <!--<v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>-->
        <v-toolbar-title>
          <router-link to="/home" style="text-decoration: none; color: black;">
            <v-icon medium color="black">home</v-icon>
            смартЭЦП
          </router-link>
        </v-toolbar-title>
        <v-spacer></v-spacer>
        <!--<v-toolbar-items>-->
          <!--<v-btn active-class="" flat icon small to="/requests" class="ml-3" v-show="isLogged()">-->
            <!--<v-badge-->
              <!--color="primary"-->
              <!--right-->
              <!--overlap-->
              <!--small-->
            <!--&gt;-->
              <!--&lt;!&ndash;<template slot="badge" v-if="notifications > 0">{{ notifications }}</template>&ndash;&gt;-->
              <!--<template slot="badge">3</template>-->
              <!--<v-icon-->
                <!--color="grey lighten-1"-->
                <!--large-->
              <!--&gt;-->
                <!--mail-->
              <!--</v-icon>-->
            <!--</v-badge>-->
          <!--</v-btn>-->
          <!--<v-btn active-class="" flat icon small to="/personal" class="ml-3" v-show="isLogged()">-->
            <!--<v-icon-->
              <!--color="grey lighten-1"-->
              <!--large-->
            <!--&gt;-->
              <!--person-->
            <!--</v-icon>-->
          <!--</v-btn>-->
          <!--<v-btn  flat icon small to="#" class="ml-3" @click="logout()" v-show="isLogged()">-->
            <!--<v-icon-->
              <!--color="grey lighten-1"-->
              <!--large-->
            <!--&gt;-->
              <!--exit_to_app-->
            <!--</v-icon>-->
          <!--</v-btn>-->
        <!--</v-toolbar-items>-->
      </v-toolbar>
      <v-content>
        <router-view @viewNotifs="notifications = 0" />
      </v-content>
    </v-app>
  </div>
</template>

<script>
export default {
  name: 'App',
  data: () => ({
    dark: false,
    drawer: false,
    notifications: 0
  }),
  created () {
    this.init()
  },
  methods: {
    isLogged: function () {
      return localStorage.getItem('login') != null
    },
    logout: function () {
      localStorage.removeItem('login')
      localStorage.removeItem('id')
      this.$router.go(0)
    },
    init () {
      this.$http.get('https://api.saject.ru/getNotifications.php?user_id=' + localStorage.getItem('id'))
        .then(response => {
          this.notifications = response.data.notifications
          console.log(this.notifications)
        })
    }
  }
}
</script>
