<script>
import commonMixin from '@/base/mixins/common.js'
import bindEvents from '@/base/bindEvent.js'
import {createPoint} from '@/base/factory.js'

export default {
  name: 'bm-polyline',
  render () {},
  mixins: [commonMixin('overlay')],
  props: {
    path: {
      type: Array
    },
    strokeColor: {
      type: String
    },
    strokeWeight: {
      type: Number
    },
    strokeOpacity: {
      type: Number
    },
    strokeStyle: {
      type: String
    },
    massClear: {
      type: Boolean,
      default: true
    },
    clicking: {
      type: Boolean,
      default: true
    },
    editing: {
      type: Boolean,
      default: false
    }
  },
  watch: {
    path: {
      handler (val, oldVal) {
        this.reload()
      },
      deep: true
    },
    strokeColor (val) {
      this.originInstance.setStrokeColor(val)
    },
    strokeOpacity (val) {
      this.originInstance.setStrokeOpacity(val)
    },
    strokeWeight (val) {
      this.originInstance.setStrokeOpacity(val)
    },
    strokeStyle (val) {
      this.originInstance.setStrokeStyle(val)
    },
    editing (val) {
      val ? this.originInstance.enableEditing() : this.originInstance.disableEditing()
    },
    massClear (val) {
      val ? this.originInstance.enableMassClear() : this.originInstance.disableMassClear()
    },
    clicking (val) {
      this.reload()
    }
  },
  methods: {
   load () {
      alert(111)
      console.log(iconss)
      const {BMap, map, path, strokeColor, strokeWeight, strokeOpacity, strokeStyle, editing, massClear, clicking} = this;
      let sy = new BMap.Symbol(BMap_Symbol_SHAPE_BACKWARD_OPEN_ARROW, {
        scale: 0.6,//图标缩放大小
        strokeColor:'#000',//设置矢量图标的线填充颜色
        strokeWeight: '12',//设置线宽
      });
      let iconss = new BMap.IconSequence(sy,'10','30');
      const overlay = new BMap.Polyline(path.map(item => createPoint(BMap, {lng: item.lng, lat: item.lat})), {
        strokeColor,
        strokeWeight,
        strokeOpacity,
        strokeStyle,
        icons:[iconss],
        enableEditing: editing,
        enableMassClear: massClear,
        enableClicking: clicking
      })
      this.originInstance = overlay
      map.addOverlay(overlay)
      bindEvents.call(this, overlay)
    }
  }
}
</script>
