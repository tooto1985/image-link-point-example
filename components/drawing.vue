<template>
  <div class="drawing">
    <img :src="$props.src" ref="img">
    <template v-if="$props.pointdata && $props.pointdata.data && $props.pointdata.data.length">
    <a v-for="(item, index) in $props.pointdata.data" :key="index"
      :style="setPoint(item)"
      :href="item.url"
      :data-tip="item.tip"
      :data-right="item.right"
      :target="item.target ? '_blank': false"
      >+</a>
    </template>
  </div>
</template>

<script>
module.exports = {
  data: function() {
    return {
      originWidth: 0,
      originHeight: 0
    };
  },
  props: {
    src: {
      type: String
    },
    pointdata: {
      type: Object
    }
  },
  mounted: function() {
    this.$refs.img.onload = () => {
      this.originWidth = this.$refs.img.naturalWidth;
      this.originHeight = this.$refs.img.naturalHeight;
    };
  },
  methods: {
    setPoint: function(item) {
      if (!item.x || !item.y) return null;
      return {
        left: 100 * (item.x / this.originWidth) + "%",
        top: 100 * (item.y / this.originHeight) + "%"
      };
    }
  }
};
</script>

<style scoped>
.drawing {
  position: relative;
  font-size: 0;
  display: inline-block;
}

.drawing > img {
  width: 100%;
}

.drawing > a {
  display: none;
}

.drawing > a[style]:not([style=""]) {
  width: 20px;
  height: 20px;
  background: red;
  display: block;
  position: absolute;
  border-radius: 20px;
  margin-left: -10px;
  margin-top: -10px;
  color: white;
  text-align: center;
  line-height: 20px;
  font-size: initial;
  text-decoration: none;
}

.drawing > a:hover:after {
  content: attr(data-tip);
  display: inline-block;
  position: absolute;
  background-color: gray;
  padding: 5px;
  border-radius: 10px;
  white-space: nowrap;
}

.drawing > a[data-right]:hover:after {
  right: 20px;
}
</style>
