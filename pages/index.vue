<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-flex
      xs12
      sm8

    >
      <v-card min-width="400">
      <v-snackbar
      v-model="snackbar"
      top
      :timeout="6000"
    >
      {{ message }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="pink"
          text
          v-bind="attrs"
          @click="snackbar = false"
        >
          закрыть
        </v-btn>
      </template>
    </v-snackbar>

          <v-card-title>

            <h1> Nuxt chat</h1></v-card-title>
          <v-card-text>
          <v-form
    ref="form"
    v-model="valid"
    lazy-validation
  >
    <v-text-field
      v-model="name"
      :counter="16"
      :rules="nameRules"
      label="ваше имя"
      required
    ></v-text-field>

    <v-text-field
      v-model="room"
      :rules="roomRules"
      label="введите комнату"
      required
    ></v-text-field>
    <v-btn
      :disabled="!valid"
      color="primary"

      @click="submit"
    >
      Validate
    </v-btn>
  </v-form>
  </v-card-text>

      </v-card>
      <v-btn @click="message">
        New message
      </v-btn>
    </v-flex>
  </v-layout>
</template>

// <script>


// export default {

//   methods:{
//     message(){
//       this.$socket.emit('createMessage',{
//         text:'from client'
//       })
//     }
//   }
// }
// </script>
<script>
  import {mapMutations} from 'vuex'
  export default {
    sockets: {
    connect(){

    }
  },
  layout:'empty',
  head:{
    title:'добро пожаловать в nuxt'
  },
  mounted(){
    const {message}=this.$router.query
    if(message==='noUser'){
      this.message="введите данные"
    }else if(message==="leftChat"){
      this.message="вы вышли из чата"
    }
    this.snackbar=!!this.message
     },
    data: () => ({
      message:'',
      valid: true,
      snackbar:false,
      name: '',
      nameRules: [
        v => !!v || 'введите имя',
        v => (v && v.length <= 10) || 'имя не должно превышать 16 символов',
      ],
      room: '',
      roomRules: [
        v => !!v || 'введите комнату'
        ],
      }),

    methods: {
      ...mapMutations(['setUser']),
      submit () {
        if(this.$refs.form.validate()){
          const user={
            name:this.name,
            room:this.room
          };
          this.$socket.emit('userJoined',user,(data)=>{
            if(typeof data === 'string'){

            }else{
              user.id=data.userId
              this.setUser(user)
              this.router.push('/chat')
            }
          })

        }
    },
  }
</script>
