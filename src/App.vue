<template>
  <div class="app">
    <div v-if="state.account.id">
      로그인되었습니다. {{state.account.name}} 님
    </div>
    <div v-else>
      <span>아이디</span>
      <input type="text" id="id" v-model="state.form.name" />
      <span>비밀번호</span>
      <input type="password" id="pw" v-model="state.form.pw" />
      <hr/>
      <button @click="submit()">로그인</button>
    </div>
  </div>
</template>

<script>
import { reactive } from "vue"
import axios from 'axios';

export default {
  setup(){
    const state = reactive({
      account: {
        id : null,
        name : ''
      },
      form:{
        name : '',
        pw : ''
      }
      
    });

    const submit = () => {
      const args = {
          name : state.form.name,
          pw : state.form.pw
      };
      axios.post('/api/user', args).then((res)=>{
        //console.log(res);
        state.account = res.data;
      });

    }

    axios.get('/api/user').then((res)=>{
      state.account = res.data;
    });

    return {state, submit};
  }
}
</script>