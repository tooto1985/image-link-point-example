<template>
  <div class="drawing" :style="{width: setWidth()}">
    <img :src="setSrc()" ref="img">
    <template v-if="setShow()">
    <a v-for="(item, index) in json.data" :key="index"
      :style="setPoint(item)"
      :href="setHref(item.url)"
      :data-tip="item.tip"
      :data-right="item.right"
      :target="item.target ? '_blank': false"
      @click="clickMobile(item, $event)"
      >+</a>
    </template>
  </div>
</template>

<script>
module.exports = {
  data: function() {
    return {
      originWidth: 0,
      originHeight: 0,
      json: this.$props.detail,
      isMobile: this.$props.mobile,
      cssWidth: this.$props.width,
      imgSrc: this.$props.src
    }
  },
  props: {
    src: {
      type: String
    },
    width: {
      type: String
    },
    detail: {
      type: Object
    },
    mobile: {
      type: Boolean
    }
  },
  mounted: function() {
    this.$refs.img.onload = () => {
      this.originWidth = this.$refs.img.naturalWidth
      this.originHeight = this.$refs.img.naturalHeight
    }
  },
  methods: {
    setWidth: function() {
      if (this.cssWidth) {
        if (/\d$/.test(this.cssWidth)) {
          return this.cssWidth + 'px'
        } else {
          return this.cssWidth
        }
      } else {
        return this.originWidth + 'px'
      }
    },
    setSrc: function() {
      return this.json && this.json.image || this.imgSrc
    },
    setShow: function() {
      return this.json && this.json.data && this.json.data.length
    }, 
    setPoint: function(item) {
      if (!item.x || !item.y) return null
      return {
        left: 100 * (item.x / this.originWidth) + "%",
        top: 100 * (item.y / this.originHeight) + "%"
      }
    },
    setHref: function(url) {
      if (!this.isMobile) return url
    },
    clickMobile: function(item, e) {
      if (this.isMobile) {
        this.json.data.filter(function(a) {
          return a != item
        }).forEach(function(a) {
          a.isMobileClicked = false
        })
        if (!item.isMobileClicked) {
          item.isMobileClicked = true
        } else {
          location.href = item.url
        }
        e.preventDefault()
      }
    }
  }
}
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
