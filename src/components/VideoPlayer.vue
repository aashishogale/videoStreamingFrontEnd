<template>
    <div>
<video width="1400"   :key="video"
controls>
  <source  :src="video" type="video/mp4">
  <source src="mov_bbb.ogg" type="video/ogg">
</video>

    <div> {{video}}</div>
</div>
  
</template>
<script>

export default {
  props:[
    'video'
  ],
   components: {
  }
    
}
</script>


<style scoped>

</style>