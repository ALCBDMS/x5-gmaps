<script>
import { gmaps } from '../init.js'

// eslint:
export default {
  name: 'gmapsCircle',
  inject: ['getMap', 'handleError'],
  props: {
    center: { type: Object, required: true },
    clickable: { type: Boolean, default: true },
    draggable: { type: Boolean, default: false },
    editable: { type: Boolean, default: false },
    fillColor: { type: String, default: 'black' },
    fillOpacity: { type: [String, Number], default: 0.3 },
    radius: { type: [String, Number], required: true },
    strokeColor: { type: String, default: 'black' },
    strokeOpacity: { type: [String, Number], default: 1 },
    strokePosition: { type: [String, Number], default: 0 },
    strokeWeight: { type: [String, Number], default: 3 },
    visible: { type: Boolean, default: true },
    zIndex: { type: [String, Number], default: 0 }
  },
  data: (vm) => ({
    circle: null,
    tempCenter: vm.center,
    tempRadius: vm.radius
  }),
  computed: {
    _options: (vm) => ({
      center: vm.center,
      clickable: vm.clickable,
      draggable: vm.draggable,
      editable: vm.editable,
      fillColor: vm.fillColor,
      fillOpacity: +vm.fillOpacity,
      radius: +vm.radius,
      strokeColor: vm.strokeColor,
      strokeOpacity: +vm.strokeOpacity,
      strokePosition: +vm.strokePosition,
      strokeWeight: +vm.strokeWeight,
      visible: vm.visible,
      zIndex: vm.zIndex
    })
  },
  methods: {
    changedCenter() {
      // This is fired when the component is replaced and may not have a tempCenter
      const oldCenter = this.tempCenter || -1
      const newCenter = this.circle.getCenter().toJSON()
      if (Math.abs(newCenter.lat - oldCenter.lat) > 0.0001 || Math.abs(newCenter.lng - oldCenter.lng) > 0.0001) {
        this.tempCenter = newCenter
        this.$emit('center-changed', newCenter)
        // TODO: Remove in major release
        this.$emit('centerChanged', newCenter) // eslint-disable-line
      }
    },
    changedRadius() {
      // This is fired when the component is replaced and may not have a tempCenter
      const oldRadius = this.tempRadius || -1
      const newRadius = this.circle.getRadius()
      if (Math.abs(newRadius - oldRadius) > 1) {
        this.tempRadius = newRadius
        this.$emit('radius-changed', newRadius)
        // TODO: Remove in major release
        this.$emit('radiusChanged', newRadius) // eslint-disable-line
      }
    }
  },
  mounted() {
    gmaps()
      .then((GMaps) => {
        this.circle = new GMaps.Circle({
          map: this.getMap(),
          options: { ...this._options }
        })
      })
      .then(() => this.circle.addListener('center_changed', () => this.changedCenter()))
      .then(() => this.circle.addListener('radius_changed', () => this.changedRadius()))
      .then(() => this.circle.addListener('click', (e) => this.$emit('click', e)))
      .then(() => this.circle.addListener('dblclick', (e) => this.$emit('double-click', e)))
      .then(() => this.circle.addListener('drag', (e) => this.$emit('drag', e.latLng.toJSON())))
      .then(() => this.circle.addListener('dragend', (e) => this.$emit('drag-end', e.latLng.toJSON())))
      .then(() => this.circle.addListener('dragstart', (e) => this.$emit('drag-start', e.latLng.toJSON())))
      .then(() => this.circle.addListener('mouseover', (e) => this.$emit('mouseover', e)))
      .then(() => this.circle.addListener('rightclick', (e) => this.$emit('right-click', e)))
      // TODO: Remove in major release
      .then(() => this.circle.addListener('dblclick', (e) => this.$emit('doubleClick', e))) // eslint-disable-line
      .then(() => this.circle.addListener('dragend', (e) => this.$emit('dragEnd', e.latLng.toJSON()))) // eslint-disable-line
      .then(() => this.circle.addListener('dragstart', (e) => this.$emit('dragStart', e.latLng.toJSON()))) // eslint-disable-line
      .then(() => this.circle.addListener('rightclick', (e) => this.$emit('rightClick', e))) // eslint-disable-line
      .catch((e) => this.handleError(e))
  },
  watch: {
    _options(newVal) {
      this.circle.setOptions(newVal)
    }
  },
  beforeDestroy() {
    if (this.circle) this.circle.setMap(null)
  },
  render: () => null
}
</script>
