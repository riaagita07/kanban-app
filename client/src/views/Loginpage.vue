<template>
  <div class="page">
    <div class="pad">
    <Loginform @logged="logged" @dontHaveAcc="dontHave" v-if="haveAcc"></Loginform>
    <Registerform @logged="logged" @haveAcc="have" v-if="!haveAcc" />
    </div>
  </div>
</template>
<script>
import Loginform from '../components/Loginform'
import Registerform from '../components/Registerform'

export default {
  name: 'Loginpage',
  components: {
    Loginform,
    Registerform
  },
  data () {
    return {
      haveAcc: true
    }
  },
  methods: {
    have () {
      this.haveAcc = true
    },
    dontHave () {
      this.haveAcc = false
    },
    logged () {
      this.$emit('logged')
    }
  }
}
</script>
<style scoped>

.page{
 width: 100%;
 height: inherit;
}

.pad{
  margin-top: 100px;
}

</style>
