<template>
  <div class="toastModal" :class="{'bg-danger': Type === 'D', 'bg-success': Type === 'S'}">
      <div class="text-light">
        <b>{{Title}}</b>
        <p>{{Desc}}</p>
      </div>
  </div>
   
</template>

<script>
export default{
    props:{
      Title: {
            type: String,
            resquired: true
        },
        Desc: {
            type: String,
            resquired: true
        },
        Type: {
            type: String,
            resquired: true
        },
    }
}
</script>