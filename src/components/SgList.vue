<template>
  <div class="suggestions rounded">
    <ul class="list p-0 m-0">
      <li 
        v-on:click="passSearchUrl(item.url)"  
        :class="[list.indexOf(item) === selectedItem ? 'hover' : '', 'item p-3 d-flex flex-column justify-content-center']" 
        v-for="item in list" 
        :key="item.id"
        @mouseover="this.$emit('hoverItem', list.indexOf(item))"
        @mouseleave="this.$emit('hoverItem', null)"
      >
        <p class="m-0">{{`${item.name}, ${item.country}`}}</p>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Suggestions',
  props: {
    list: Object,
    selectedItem: Number,
  },
  methods: {
    passSearchUrl: function(url) {
      this.$emit('locationClick', url);
    },
  },
  unmounted() {
    this.$emit('componentDismount');
  }
}
</script>

<style scoped>
#input-group {
  position: relative;
}
.suggestions {
  position: absolute;
  width: 82%;
  top: 38px;
  z-index: 99;
  background: white;
  border: 1px solid rgb(199, 199, 199);
}
.suggestions .list {
  list-style-type: none;
}
.suggestions .item {
  cursor: pointer;
}
.suggestions .item.hover {
  background: rgb(189, 189, 189) !important;
}

</style>