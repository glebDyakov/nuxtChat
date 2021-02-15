<template>
<v-flex xs12>
          <v-text-field
            v-model="text"
            label="введите сообщение"
            outline

            @keydown.enter="send"
          ></v-text-field>
        </v-flex>
</template>
<script>
export default {
  date:()=>({
    text:''
  }),
  methods:{
    send(){
      this.$socket.emit('createMessage',{
        text:this.text,
        id:this.$store.state.user.id
      },(data)=>{
        if(typeof data==='string'){

        }else{
          this.text=''
        }
      })
    }
  }
}
</script>
