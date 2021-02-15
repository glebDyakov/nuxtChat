<template>
<v-app app dark>
  <v-navigation-drawer v-model="drawer" app mobile-break-point="650">
        <v-list subheader>
      <v-subheader>список людей в комнате</v-subheader>

      <v-list-item
        v-for="u in users"
        :key="u.id"
        @click.prevent=""
      >
        <v-list-item-content>
          <v-list-item-title>{{u.name}}</v-list-item-title>
        </v-list-item-content>

        <v-list-item-icon>
          <v-icon :color="u.id===user.id ? 'primary' : 'grey'">chat_bubble</v-icon>
        </v-list-item-icon>
      </v-list-item>
    </v-list>


  </v-navigation-drawer>

  <v-toolbar-bar app>
    <v-toolbar-side-icon @click="drawer=!drawer"></v-toolbar-side-icon>
    <v-btn icon @click="exit">
     <v-icon>arrow_back</v-icon>
    </v-btn>
    <v-toolbar-title>чат комнаты {{user.room}}</v-toolbar-title>

    <!-- -->
  </v-toolbar-bar>

  <!-- Sizes your content based upon application components -->
  <v-content>

    <!-- Provides the application the proper gutter -->
    <div style="height:100%">

      <!-- If using vue-router -->
      <nuxt/>
    </div>
  </v-content>

  <v-footer app>
    <!-- -->
  </v-footer>
</v-app>

</template>

<script>
import {mapState, mapMutations} from 'vuex'
export default {
  data:()=>({
    drawer:true,
    // user:[
    //   {id:1,name:'user 1'},
    //   {id:2,name:'user 2'},
    // ]
  }),
  computed:mapState(['user','users']),
  methods:{
    ...mapMutations(['clearData']),
    exit(){
      this.$socket.emit('userLeft',this.user.id,()=>{
      this.$router.push('/?message=leftChat')
      this.clearData()
      })

    }
  }
}
</script>
