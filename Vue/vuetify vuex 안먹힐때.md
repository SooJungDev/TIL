## vuetify에서 vuex 작동안할때
- vuetify 에 v-dialog 사용시 v-model에 걸린 mapState가 작동하지 않는 문제가 생겼었음
- 인터넷 검색해보니 해결방법이 나와있었음
~~~ javascript
<template>
  <div>
        <v-checkbox
          :label="`Admin`"
          :value="registerAdmin"
          @click="setRegisterAdmin"
        ></v-checkbox>
        <v-btn color="green" dark @click="register">
          <v-icon class="mr-2">account_circle</v-icon>
          Register
        </v-btn>
  </div>
</template>
<script>
import { mapState, mapMutations, mapActions } from 'vuex';

export default {
  computed: {
    ...mapState('authentication', [
      'registerAdmin',
    ]),
  },
  methods: {
    ...mapMutations('authentication', [
      'setRegisterAdmin',
    ]),
    ...mapActions('authentication', [
      'register',
    ]),
  },
};
</script>
~~~

- **밑에처럼 작성해주면됨 해결방법 computed에 get(), set()을 걸어줌**
~~~ javascript
<v-checkbox v-model="isAdmin"/>
...
computed: {
  isAdmin: {
    get() {
      return this.rigisterAdmin;
    },
    set(value) {
       this.setRegisterAdmin(value);
    }
  }
}
~~~

## 참고사이트
  - [vuetify에서 vuex 작동안할때](https://forum.vuejs.org/t/help-with-vuetify-vuex-checkbox/35765)
