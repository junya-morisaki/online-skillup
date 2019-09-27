<template>

  <div class='msg'>

    <div class='name'>
      {{$props.name}}
    </div>
    <div class='text'>
       {{ $props.message }}
    </div>
    <div class='time'>
      {{$props.time}}
    </div>
  </div>

</template>

<script>
// import VueTypes from 'vue-types';

export default {
  // props: {
  //   message: VueTypes.Array.isRequired
  // }
  props: ['message', 'name', 'active', 'time']
};
</script>

<style lang="scss" scoped>
.msg {
  font-size: 15px;
  margin-bottom: 25px;
}

.name {
  font-size: 0.5em;
  color: #fff;
}

.text {
  font-size: 1em;
  margin: 0.5em;
  padding: 0.3em;
  border: solid 1px;
  border-radius: 0.5em;
  background-color: #fff;
}

.time {
  font-size: 0.5em;
  color: #fff;
}
</style>
