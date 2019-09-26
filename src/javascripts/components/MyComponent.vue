<template>

  <div class='msg'>

    <div class='name'>
      {{$props.name}}
    </div>
    <div class='message'>
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
  font-size: 10px;
  margin-bottom: 10px;
}

.name {
  font-size: 0.5em;
}

.message {
  font-size: 1.5em;
  margin: 0.5em;
  padding: 0.3em;
  border: solid 1px;
  border-radius: 0.5em;
}

.time {
  font-size: 0.3em;
}
</style>
