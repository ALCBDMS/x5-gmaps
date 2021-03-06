<template>
  <example-wrapper title="Info Windows">
    <div class="instructions">
      <p class="instructions-title">
        While the close button is built into InfoWindows, you can listen to a close event (@closed) and access the open function (using Vue refs) to re-open.
      </p>
      <span>Close the second InfoWindow and re-open it here:</span>
      <button class="instructions-action" :disabled="!bClosed" @click="openB">Re-open</button>
    </div>
    <template #description>
      <p>
        As with markers, there are also a few
        <a href="https://developers.google.com/maps/documentation/javascript/reference/info-window#InfoWindowOptions" target="_blank">options</a> available for InfoWindows
      </p>
      <p>(they also require a position option)</p>
    </template>
    <template #map>
      <gmaps-map :options="mapOptions">
        <gmaps-info-window :options="optionsA">
          <p>Any <span style="background:yellow;">HTML</span> can<br />go in <strong>these</strong>.</p>
        </gmaps-info-window>
        <gmaps-info-window ref="b" :options="optionsB" style="background:#BBF0FF;" @closed="bClosed = true">
          <p>Even a whole Vue component<img src="../img/marker2.png" height="20px" /></p>
          <p><em>(but they're locked in the white Google bubble with a close button)</em></p>
        </gmaps-info-window>
      </gmaps-map>
    </template>
    <template #code>
      <div>
        <pre>
&lt;gmaps-map>
  &lt;gmaps-info-window :options="optionsA">
    &lt;p>
      Any &lt;span style="background:yellow;">HTML&lt;/span> can&lt;br />go in
      &lt;strong>these&lt;/strong>.
    &lt;/p>
  &lt;/gmaps-info-window>

  &lt;gmaps-info-window
    ref="b"
    :options="optionsB"
    style="background:#BBF0FF;"
    @closed="bClosed = true">
    &lt;p>Even a whole Vue component&lt;img src="./marker2.png" height="20px" />&lt;/p>
    &lt;p>&lt;em>(but they're locked in the white Google bubble with a close button)&lt;/em>&lt;/p>
  &lt;/gmaps-info-window>
&lt;/gmaps-map>

...

optionsA = { position: { lat: -27.46, lng: 153.02 } }
optionsB = { position: { lat: -27.5, lng: 153.02 } }

...

openB() {
  this.$refs.b.open()
}
          </pre
        >
      </div>
    </template>
  </example-wrapper>
</template>

<script>
import ExampleWrapper from '../Wrapper'
import { gmapsMap, gmapsInfoWindow } from '@/' // import from 'x5-gmaps' for plugin

export default {
  name: 'ExampleInfoWindow',
  components: { ExampleWrapper, gmapsMap, gmapsInfoWindow },
  data: () => ({
    mapOptions: {
      center: { lat: -27.47, lng: 153.025 },
      zoom: 12,
      fullscreenControl: false,
      mapTypeControl: false,
      rotateControl: false,
      scaleControl: false,
      streetViewControl: false,
      zoomControl: false
    },
    optionsA: {
      position: { lat: -27.46, lng: 153.02 }
    },
    optionsB: {
      position: { lat: -27.5, lng: 153.02 }
    },
    bClosed: false
  }),
  methods: {
    openB() {
      this.$refs.b.open()
    }
  }
}
</script>
