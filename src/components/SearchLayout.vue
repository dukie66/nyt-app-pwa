<template>
  <div class="row">
    <div class="col-12">
      <slot name="top"></slot>
    </div>
    <div class="col-12">
      <slot name="bottom"></slot>
    </div>
  </div>
</template>
<script>
export default {
  name: "SearchLayout"
};
</script>