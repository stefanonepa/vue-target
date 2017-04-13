<template>
  <div>
    <svg xmlns:dc="http://purl.org/dc/elements/1.1/"
         xmlns:cc="http://web.resource.org/cc/"
         xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
         xmlns:svg="http://www.w3.org/2000/svg"
         xmlns="http://www.w3.org/2000/svg"
         version="1.0"
         :width="size"
         :height="size"
         style="border: 1px solid black;"
         id="target" >
      <metadata id="metadata18">
        <rdf:RDF>
          <cc:Work rdf:about="">
            <dc:format>image/svg+xml</dc:format>
            <dc:type rdf:resource="http://purl.org/dc/dcmitype/StillImage" />
            <dc:title>Official FITA 80cm Archery Target</dc:title>
            <dc:date>2006-06-01</dc:date>
            <dc:creator>
              <cc:Agent>
                <dc:title>Alberto Barbati</dc:title>
              </cc:Agent>
            </dc:creator>
            <cc:license rdf:resource="http://creativecommons.org/licenses/by-sa/2.0/" />
            <dc:subject>
              <rdf:Bag>
                <rdf:li>archery target</rdf:li>
              </rdf:Bag>
            </dc:subject>
            <dc:description>An official FITA 80cm archery target. Real colors are: Yellow: Pantone 107U Red: Pantone 032U Light blue: Pantone 306U</dc:description>
          </cc:Work>
          <cc:License rdf:about="http://creativecommons.org/licenses/by-sa/2.0/">
            <cc:permits rdf:resource="http://web.resource.org/cc/Reproduction" />
            <cc:permits rdf:resource="http://web.resource.org/cc/Distribution" />
            <cc:requires rdf:resource="http://web.resource.org/cc/Notice" />
            <cc:requires rdf:resource="http://web.resource.org/cc/Attribution" />
            <cc:permits rdf:resource="http://web.resource.org/cc/DerivativeWorks" />
            <cc:requires rdf:resource="http://web.resource.org/cc/ShareAlike" />
          </cc:License>
        </rdf:RDF>
      </metadata>
      <mycircle v-for="currCir in revCircles" :key="currCir"
                :containerSize="size" :val="currCir.val" :percentage="currCir.percentage"
                :fillColor="currCir.fillColor" :stroke="currCir.stroke"
                @pull="pull" @updateChoose="updateChoose"></mycircle>

      <path :d="crossCoordinate"
            stroke="black" stroke-width="0.2"
            id="Center" />

      <circle v-for="arrow in arrows" :cx="arrow.x" :cy="arrow.y" :r="size*0.009" stroke="green" fill="#3a3a3a" stroke-width="1"
              @mouseup.stop="pull(arrow.val, $event)"
              @mousemove.stop="updateChoose(arrow.val)"/>
    </svg>
    <div>
      <h1>Current Value: {{currentVal}} </h1>
      <h1>Choose Value: {{finalVal}} </h1>
      <p>arrows {{arrows}}</p>
    </div>
  </div>
</template>

<script>
import mycircle from './MyCircle'

export default {
  name: 'target',
  props: ['size'],
  components: { mycircle },
  data () {
    return {
      currentVal: '',
      finalVal: '',
      arrows: [],
      circles: [
        { fillColor: '#fff535', val: 'x', percentage: 0.01, stroke: 'black' },
        { fillColor: '#fff535', val: 10, percentage: 0.05, stroke: 'black' },
        { fillColor: '#fff535', val: 9, percentage: 0.1, stroke: 'black' },
        { fillColor: '#fd1b14', val: 8, percentage: 0.15, stroke: 'black' },
        { fillColor: '#fd1b14', val: 7, percentage: 0.2, stroke: 'black' },
        { fillColor: '#41b7c8', val: 6, percentage: 0.25, stroke: 'black' },
        { fillColor: '#41b7c8', val: 5, percentage: 0.3, stroke: 'black' },
        { fillColor: 'black', val: 4, percentage: 0.35, stroke: 'white' },
        { fillColor: 'black', val: 3, percentage: 0.4, stroke: 'white' },
        { fillColor: 'white', val: 2, percentage: 0.45, stroke: 'black' },
        { fillColor: 'white', val: 1, percentage: 0.5, stroke: 'black' },
        { fillColor: 'white', val: 'M', percentage: 1, stroke: 'white' }
      ]
    }
  },
  computed: {
    revCircles () {
      return this.circles.reverse()
    },
    crossCoordinate () {
      return `M ${this.center - (this.size * 0.01)} ${this.center} L ${this.center + (this.size * 0.01)} ${this.center}
              M ${this.center} ${this.center - (this.size * 0.01)}  L ${this.center} ${this.center + (this.size * 0.01)}`
    },
    center () {
      return this.size / 2
    }
  },
  methods: {
    pull (value) {
      // Calculation of the coordinates in the svg
      var svg = document.querySelector('svg')
      var pt = svg.createSVGPoint()
      pt.x = value.e.clientX
      pt.y = value.e.clientY
      var loc = pt.matrixTransform(svg.getScreenCTM().inverse())

      this.finalVal = value.circle.val
      this.arrows.push({val: value.circle.val, x: loc.x.toFixed(0), y: loc.y.toFixed(0)})
    },
    updateChoose (value) {
      this.currentVal = value.circle.val
    }
  }
}
</script>
