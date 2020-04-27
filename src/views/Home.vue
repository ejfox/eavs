<template>
  <section class="avenir">
    <div class="f5 f3-ns lh-copy">
    <div class="step measure">
      <h2 class="lh-copy measure">One of the ways we can understand how Americans vote is EAVS data – scroll down for an overview of <span class="pa2 br2" style="background-color: #FFB915;">OSET's analysis of EAVS Voter Removal data</span></h2>
    </div>
    <div id="sankey" class="sticky" v-show="step < 14">
      <!-- <img src="image/eavs-sankey-01.svg" /> -->
      <Sankey :step="step" />
    </div>
    <div class="step measure">
      <h2><span class="pa1 bg-white br2">17,300,470 Americans</span> were removed from voter records in the EAVS data analyzed</h2>
    </div>
    <div class="step measure">
      <h2><span class="pa1 bg-white br2">3,730,617 Americans</span> were removed from voter rolls because they died, <span class="o-70">an inarguable reason why they should no longer be allowed to vote</span></h2>
    </div>
    <div class="step measure">
      <h2 class="lh-title">Voters can also be removed for other standard reasons</h2>
      <ul>
        <li>Moved outside jurisdiction <small class="pa1 bg-white br2 f5">(4.27 million voters)</small></li>
        <li>Voter requested to be removed themselves <small class="pa1 bg-white br2 f5">(489K voters)</small></li>
        <li>The voter had a disqualifying event like a felony conviction <small class="pa1 bg-white br2 f5">(394K)</small></li>
        <li>The voter has been declared mentally incompetent <small class="pa1 bg-white br2 f5">(Only 9,569 across the US)</small></li>
      </ul>
    </div>
    <div class="step measure">
      <h2>But there are <span class="pa1 br2" style="background-color: #FFB915;">other reasons</span> that are a little more questionable that make up for <strong class="pa1 bg-white br2">7,305,854 removals</strong></h2>
    </div>
    <div class="step measure">
      <h2>Over 6 million Americans were removed because they <span class="pa1 br2" style="background-color: #FFB915;">"failed to respond"</span> to mail informing them their registration was in question</h2>
    </div>
    <div class="step measure">
      <h2 class="lh-title">The 10 states with the most removals were:</h2>
      <ul class="list ma0 pa0 b">
        <li class="mb2">
          <div class="w-50 dib v-top">Texas</div>
          <div class="dib w-50 pa1 bg-white br2">1,609,040 voters removed</div>
        </li>
        <li class="mb2">
          <div class="w-50 dib v-top">Indiana</div>
          <div class="dib w-50 pa1 bg-white br2">1,292,252 voters removed</div>
        </li>
        <li class="mb2">
          <div class="w-50 dib v-top">Florida</div>
          <div class="dib w-50 pa1 bg-white br2">1,046,514 voters removed</div>
        </li>
        <li class="mb2">
          <div class="w-50 dib v-top">Virginia</div>
          <div class="dib w-50 pa1 bg-white br2">926,015 voters removed</div>
        </li>
        <li class="mb2">
          <div class="w-50 dib v-top">Illinois</div>
          <div class="dib w-50 pa1 bg-white br2">866,679 voters removed</div>
        </li>
        <!-- <li class="mb2">
          <div class="w-50 dib v-top">Georgia </div>
          <div class="dib w-50 pa1 bg-white br2">797,124 voters removed</div>
        </li>
        <li class="mb2">
          <div class="w-50 dib v-top">North Carolina</div>
          <div class="dib w-50 pa1 bg-white br2">744,453 voters removed</div>
        </li>
        <li class="mb2">
          <div class="w-50 dib v-top">California</div>
          <div class="dib w-50 pa1 bg-white br2">733,618 voters removed</div>
        </li>
        <li class="mb2">
          <div class="w-50 dib v-top">Massachusetts</div>
          <div class="dib w-50 pa1 bg-white br2">629,710 voters removed</div>
        </li>
        <li class="mb2">
          <div class="w-50 dib v-top">Washington</div>
          <div class="dib w-50 pa1 bg-white br2">560,802 voters removed</div>
        </li> -->
      </ul>
    </div>

    <div id="map"
    :class="['sticky', step >= 14 ? 'over' : 'under']"></div>

    <div class="step measure" data-center-state="Texas">
      <h2>Let's look at Texas, which had the largest total number of voters removed in our data.</h2>

      <h2>This makes sense since Texas is the second largest state in terms of population.</h2>
    </div>

    <div class="step measure" data-center-state="Indiana">
      <h2>Indiana removed a total of 1,292,252 registered voters</h2>
    </div>
    
    <div class="step measure" data-center-state="Florida">
      <h2>Florida removed 1,046,514 registered voters</h2>
    </div>
    
    <div class="step measure" data-center-state="Virginia">
      <h2>Virginia removed 926,015 voters</h2>
    </div>

    <div class="step measure">
      <h2>But how can we put those numbers in context?</h2>
    </div>

    <div class="step measure">
      <h2>Well, it's important to know that in some counties, the vote counts are very close. Some elections are decided by votes in the single digits.</h2>
    </div>

    <div class="step measure">
      <h2>Looking at historical election results, we wanted to identify places where the number of voters removed was larger than the win margin of an election winner.</h2>

      <h2>Put simply, we wanted to find places where the number of registered voters removed would be enough to theoretically sway the election results from one party to another.</h2>
    </div>

  </div>

  </section>
</template>

<script>
import 'intersection-observer'
import scrollama from 'scrollama'
import * as d3 from 'd3'
import Chance from 'chance'
import _ from 'lodash'

// @ is an alias to /src
import Sankey from '@/components/Sankey.vue'
const chance = new Chance()

const stateCentroids = [
  {
    'State': 'Alabama',
    'Latitude': 32.806671,
    'Longitude': -86.79113
  },
  {
    'State': 'Alaska',
    'Latitude': 61.370716,
    'Longitude': -152.404419
  },
  {
    'State': 'Arizona',
    'Latitude': 33.729759,
    'Longitude': -111.431221
  },
  {
    'State': 'Arkansas',
    'Latitude': 34.969704,
    'Longitude': -92.373123
  },
  {
    'State': 'California',
    'Latitude': 36.116203,
    'Longitude': -119.681564
  },
  {
    'State': 'Colorado',
    'Latitude': 39.059811,
    'Longitude': -105.311104
  },
  {
    'State': 'Connecticut',
    'Latitude': 41.597782,
    'Longitude': -72.755371
  },
  {
    'State': 'Delaware',
    'Latitude': 39.318523,
    'Longitude': -75.507141
  },
  {
    'State': 'District of Columbia',
    'Latitude': 38.897438,
    'Longitude': -77.026817
  },
  {
    'State': 'Florida',
    'Latitude': 27.766279,
    'Longitude': -81.686783
  },
  {
    'State': 'Georgia',
    'Latitude': 33.040619,
    'Longitude': -83.643074
  },
  {
    'State': 'Hawaii',
    'Latitude': 21.094318,
    'Longitude': -157.498337
  },
  {
    'State': 'Idaho',
    'Latitude': 44.240459,
    'Longitude': -114.478828
  },
  {
    'State': 'Illinois',
    'Latitude': 40.349457,
    'Longitude': -88.986137
  },
  {
    'State': 'Indiana',
    'Latitude': 39.849426,
    'Longitude': -86.258278
  },
  {
    'State': 'Iowa',
    'Latitude': 42.011539,
    'Longitude': -93.210526
  },
  {
    'State': 'Kansas',
    'Latitude': 38.5266,
    'Longitude': -96.726486
  },
  {
    'State': 'Kentucky',
    'Latitude': 37.66814,
    'Longitude': -84.670067
  },
  {
    'State': 'Louisiana',
    'Latitude': 31.169546,
    'Longitude': -91.867805
  },
  {
    'State': 'Maine',
    'Latitude': 44.693947,
    'Longitude': -69.381927
  },
  {
    'State': 'Maryland',
    'Latitude': 39.063946,
    'Longitude': -76.802101
  },
  {
    'State': 'Massachusetts',
    'Latitude': 42.230171,
    'Longitude': -71.530106
  },
  {
    'State': 'Michigan',
    'Latitude': 43.326618,
    'Longitude': -84.536095
  },
  {
    'State': 'Minnesota',
    'Latitude': 45.694454,
    'Longitude': -93.900192
  },
  {
    'State': 'Mississippi',
    'Latitude': 32.741646,
    'Longitude': -89.678696
  },
  {
    'State': 'Missouri',
    'Latitude': 38.456085,
    'Longitude': -92.288368
  },
  {
    'State': 'Montana',
    'Latitude': 46.921925,
    'Longitude': -110.454353
  },
  {
    'State': 'Nebraska',
    'Latitude': 41.12537,
    'Longitude': -98.268082
  },
  {
    'State': 'Nevada',
    'Latitude': 38.313515,
    'Longitude': -117.055374
  },
  {
    'State': 'New Hampshire',
    'Latitude': 43.452492,
    'Longitude': -71.563896
  },
  {
    'State': 'New Jersey',
    'Latitude': 40.298904,
    'Longitude': -74.521011
  },
  {
    'State': 'New Mexico',
    'Latitude': 34.840515,
    'Longitude': -106.248482
  },
  {
    'State': 'New York',
    'Latitude': 42.165726,
    'Longitude': -74.948051
  },
  {
    'State': 'North Carolina',
    'Latitude': 35.630066,
    'Longitude': -79.806419
  },
  {
    'State': 'North Dakota',
    'Latitude': 47.528912,
    'Longitude': -99.784012
  },
  {
    'State': 'Ohio',
    'Latitude': 40.388783,
    'Longitude': -82.764915
  },
  {
    'State': 'Oklahoma',
    'Latitude': 35.565342,
    'Longitude': -96.928917
  },
  {
    'State': 'Oregon',
    'Latitude': 44.572021,
    'Longitude': -122.070938
  },
  {
    'State': 'Pennsylvania',
    'Latitude': 40.590752,
    'Longitude': -77.209755
  },
  {
    'State': 'Rhode Island',
    'Latitude': 41.680893,
    'Longitude': -71.51178
  },
  {
    'State': 'South Carolina',
    'Latitude': 33.856892,
    'Longitude': -80.945007
  },
  {
    'State': 'South Dakota',
    'Latitude': 44.299782,
    'Longitude': -99.438828
  },
  {
    'State': 'Tennessee',
    'Latitude': 35.747845,
    'Longitude': -86.692345
  },
  {
    'State': 'Texas',
    'Latitude': 31.054487,
    'Longitude': -97.563461
  },
  {
    'State': 'Utah',
    'Latitude': 40.150032,
    'Longitude': -111.862434
  },
  {
    'State': 'Vermont',
    'Latitude': 44.045876,
    'Longitude': -72.710686
  },
  {
    'State': 'Virginia',
    'Latitude': 37.769337,
    'Longitude': -78.169968
  },
  {
    'State': 'Washington',
    'Latitude': 47.400902,
    'Longitude': -121.490494
  },
  {
    'State': 'West Virginia',
    'Latitude': 38.491226,
    'Longitude': -80.954453
  },
  {
    'State': 'Wisconsin',
    'Latitude': 44.268543,
    'Longitude': -89.616508
  },
  {
    'State': 'Wyoming',
    'Latitude': 42.755966,
    'Longitude': -107.30249
  }
]

export default {
  name: 'home',
  components: {
    Sankey
  },
  data: function () {
    return {
      step: 0,
      centroids: stateCentroids
    }
  },
  mounted: function () {
    mapboxgl.accessToken = 'pk.eyJ1IjoiZWpmb3giLCJhIjoiY2lyZjd0bXltMDA4b2dma3JzNnA0ajh1bSJ9.iCmlE7gmJubz2RtL4RFzIw'

    const center = _.find(this.centroids, (d) => { return d.State === 'Texas' })
    var map = new mapboxgl.Map({
      container: 'map',
      // style: 'mapbox://styles/mapbox/streets-v9',
      style: 'mapbox://styles/ejfox/ck06vs19600921cnnume7jnid',
      zoom: 5.5,
      // center: [-95.369, 29.760]
      center: [center.Longitude, center.Latitude]
    })

    const scroller = scrollama()
    scroller.setup({
      step: '.step',
      debug: true
    })
      .onStepEnter(response => {
        this.step = response.index
        // { element, index, direction }
        console.log({ response })
        d3.select(response.element).classed('active-step', true)

        // if(response.index === 6) {
        //   d3.select('#map').classed('o-0', false)
        //   d3.select('#map').classed('o-100', true)

        //   d3.select('#sankey').classed('o-0', true)
        //   d3.select('#sankey').classed('o-100', false)
        // }

        const centerState = response.element.dataset.centerState
        let centroid = []
        if (centerState) {
          // centroid = this.centroids[centerState]
          centroid = _.find(this.centroids, ['State', centerState])
          console.log('Centroid found...', centroid)
          const gotoLocation = [centroid.Longitude, centroid.Latitude]
          map.flyTo({
            center: gotoLocation,
            speed: 0.3
          })
        }
      })
      .onStepExit(response => {
        // { element, index, direction }

        d3.select(response.element).classed('active-step', false)
      })

    window.addEventListener('resize', scroller.resize)
  },
  watch: {
    step: function (newVal, oldVal) {
      this.$router.push({ path: this.$router.path, query: { step: this.step } })
    }
  }
}
</script>
<style lang="stylus">
.step
  z-index 5
  padding 0.65em 1.65em 
  // min-height 62vh
  margin-top 33vh
  margin-bottom 33vh
  // width 60%
  margin-left auto
  margin-right auto
  color #999
  transition all 0.5s ease-out
  opacity 0.8
  // border 2px solid red
  border-radius 0.5em

  &:nth-child(1)
    margin-top 18vh

.active-step
  opacity 1
  color black !important
  background-color: rgba(255,255,255,0.5)
  text-shadow: 0.05em 0.05em 0.08em rgba(255,255,255,0.9)

#map
  font-size 14px
  // position fixed
  width 100vw
  height 100vh
  left 0
  top 0

#map.under
  z-index -1
  // border 4px solid red

#map.over
  z-index 0
  // border 4px solid blue

#sankey
  z-index -2
  top 0

.sticky
  position sticky
</style>
