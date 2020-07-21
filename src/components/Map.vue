<template>
  <div
    :class="'container ' + classString + (interactive ? ' interactive' : '')"
  >
    <div v-if="interactive" :class="{ menu: true }">
      <div
        :class="{ row: true }"
        v-for="(sublist, i) in variables"
        :key="'sublist' + i"
      >
        <div
          :class="{ button: true, active: currentVariable === v.key }"
          v-for="v in sublist"
          :key="v.key"
          @click="currentVariable = v.key"
        >
          {{ v.display }}
        </div>
      </div>
    </div>

    <MglMap
      :class="'map-test ' + classString"
      :zoom="zoom"
      :center="center"
      accessToken="pk.eyJ1IjoibG9nYW53IiwiYSI6IlQzWHJqc3cifQ.KY3j-syHXeYmI69JmLqGqQ"
      :mapStyle="'mapbox://styles/loganw/ckcoy9qw60og41hnx4mr9wn8c/draft'"
      @click="togglehover"
      v-if="!staticImage"
    >
      <MglVectorLayer
        :source="{
          url: 'mapbox://loganw.8ikir8f6',
          type: 'vector',
          promoteId: 'zip',
        }"
        before="water"
        layerId="choropleth-fill"
        sourceId="ppp"
        :layer="layerStyle"
        @mousemove="mousemove"
        @mouseleave="mouseleave"
      />
      <MglVectorLayer
        :source="{ url: 'mapbox://loganw.8ikir8f6', type: 'vector' }"
        before="water"
        layerId="choropleth-stroke"
        sourceId="ppp"
        :layer="strokeStyle"
      />
      <MglVectorLayer
        :source="{
          url: 'mapbox://loganw.8ikir8f6',
          type: 'vector',
          promoteId: 'zip',
        }"
        v-if="hoverFeature"
        layerId="choropleth-highlight"
        sourceId="ppp"
        :layer="{
          type: 'line',
          'source-layer': 'ppp_zctas',
          paint: {
            'line-color': [
              'case',
              ['boolean', ['feature-state', 'hover'], false],
              'black',
              'rgba(0,0,0,0)',
            ],
            'line-width': 1,
          },
        }"
      />
    </MglMap>
    <img v-else :src="staticImage" />

    <div
      v-if="!hideLegend"
      :class="{ legend: true, w1: true, [legendStyle]: true, interactive }"
    >
      <div class="title">{{ currentTitle }}</div>
      <div class="legend-row" v-for="(l, i) in legend" :key="'legend' + i">
        <div class="color" :style="{ backgroundColor: scheme(1 - i * 0.14) }" />
        <div class="label"><span class="ell">…</span>{{ l }}</div>
      </div>
    </div>

    <div v-if="interactive && hoverFeature" class="info w3p p-no-leading l1">
      <div class="share">
        <div class="w1p">
          <div class="zip">{{ hoverFeature.properties.zip }}</div>
          <div v-for="(sublist, i) in variables" :key="'sublist_zcta' + i">
            <div
              class="zip_row"
              v-for="v in sublist"
              :key="v.key"
              @click="currentVariable = v.key"
            >
              <div class="title">{{ v.display }}</div>
              <div class="value">
                {{
                  (v.scale === "$" ? "$" : "") +
                    numberWithCommas(
                      Math.round(v.js(hoverFeature.properties) * v.multiplier)
                    ) +
                    (v.scale !== "$" ? v.scale : "")
                }}
              </div>
            </div>
          </div>
        </div>
        <div class="w2">
          <div class="zip" v-if="hoveredLoans">
            {{ hoveredLoans.length }} large loans
          </div>
          <div
            class="zip_row"
            v-for="(loan, i) in hoveredLoans"
            :key="loan.BusinessName + i"
          >
            <div class="title">{{ loan.BusinessName }}</div>
            <div class="value">
              {{ loan.LoanRange.slice(2) }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { MglMap, MglVectorLayer } from "vue-mapbox";
import * as d3 from "d3";
import { variables, interactiveVariables } from "../constants/ranges";
import bigLoans from "../assets/loan-zips.json";
import download from "downloadjs";

export default {
  name: "Map",
  components: {
    MglMap,
    MglVectorLayer,
  },
  props: {
    center: Array,
    zoom: Number,
    classString: String,
    variable: String,
    title: String,
    legendStyle: String,
    hideLegend: Boolean,
    interactive: Boolean,
    staticImage: String,
    enableScreenshots: Boolean,
  },
  data() {
    return {
      scale: [0, 0.33, 0.4, 0.46, 0.54, 0.64, 2.25],
      currentVariable: this.variable,
      strokeStyle: {
        type: "line",
        "source-layer": "ppp_zctas",
        paint: {
          "line-color": "#888",
          "line-width": 0.5,
        },
      },
      hoverFeature: null,
      interactiveVariables,
      hoveredStateId: false,
      hovering: true,
    };
  },
  methods: {
    numberWithCommas(x) {
      return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },
    togglehover({ map }) {
      function takeScreenshot(map) {
        return new Promise(function(resolve) {
          map.once("render", function() {
            resolve(map.getCanvas().toDataURL("image/jpeg", 0.9));
          });
          /* trigger render */
          map._render();
        });
      }

      if (!this.interactive && this.enableScreenshots) {
        console.log("trying to trigger screenshot", map);

        /* example */
        takeScreenshot(map).then(function(data) {
          download(data, Date.now() + ".jpg", "image/png");
        });
      }

      this.hovering = !this.hovering;
    },
    mousemove(e) {
      if (!this.hovering) return;

      if (e.mapboxEvent.features.length > 0) {
        if (this.hoveredStateId) {
          e.map.setFeatureState(
            {
              source: "ppp",
              id: this.hoveredStateId,
              sourceLayer: "ppp_zctas",
            },
            { hover: false }
          );
        }

        this.hoveredStateId = e.mapboxEvent.features[0].id;
        e.map.setFeatureState(
          { source: "ppp", id: this.hoveredStateId, sourceLayer: "ppp_zctas" },
          { hover: true }
        );
        this.hoverFeature = e.mapboxEvent.features[0];
      } else {
        this.hoveredStateId = false;
        this.hoverFeature = null;
      }
    },
    mouseleave(e) {
      if (!this.hovering) return;

      e.map.setFeatureState(
        {
          source: "ppp",
          id: this.hoveredStateId,
          sourceLayer: "ppp_zctas",
        },
        { hover: false }
      );
      this.hoveredStateId = false;
      this.hoverFeature = null;
    },
  },
  computed: {
    variables() {
      return interactiveVariables.map((list) =>
        list.map((v) => ({ ...variables[v], key: v }))
      );
    },
    currentTitle() {
      return this.title ? this.title : variables[this.currentVariable].display;
    },
    scheme() {
      return (v) =>
        (variables[this.currentVariable].color || d3.interpolateMagma)(v);
    },
    breaks() {
      return variables[this.currentVariable].breaks;
    },
    legend() {
      let scale = variables[this.currentVariable].scale;
      return this.breaks
        .slice(1)
        .map(
          (b, i) =>
            (scale === "$" ? "$" : "") +
            this.numberWithCommas(
              this.breaks[i] * variables[this.currentVariable].multiplier
            ) +
            "–" +
            this.numberWithCommas(
              b * variables[this.currentVariable].multiplier
            ) +
            (scale !== "$" ? scale : "")
        );
    },
    hoveredLoans() {
      if (!this.hoverFeature) return [];

      return bigLoans[this.hoverFeature.properties.zip];
    },
    layerStyle() {
      let d = 0.7 / (this.breaks.length - 2);

      let mapbox = this.breaks
        .slice(1, this.breaks.length - 1)
        .reduce((acc, cur, i) => [...acc, cur, this.scheme(1 - (i + 1) * d)], [
          this.scheme(1),
        ]);

      return {
        type: "fill",
        "source-layer": "ppp_zctas",
        paint: {
          "fill-color": [
            "step",
            variables[this.currentVariable].mapbox,
            ...mapbox,
          ],
        },
      };
    },
  },
};
</script>

<style lang="scss">
@import "@/constants/_style.scss";

.map-test {
  box-sizing: content-box;
  height: 100%;
  //   opacity: 0.5;

  // * {
  //   pointer-events: none;
  // }
}

.interactive .map-test {
  @media (max-width: 928px) {
    height: calc(#{$grid} * 36) !important;
  }
}
</style>

<style lang="scss" scoped>
@import "plumber-sass/_plumber.scss";
@import "@/constants/_style.scss";

.container {
  position: relative;
  margin-bottom: $grid;
  // pointer-events: none;

  &.interactive {
    pointer-events: all;
  }

  &.below {
    margin-bottom: calc(
      1 * (#{$gutter-width} * 2 + #{$block-width} * 1) * #{$grid}
    );
  }
}

.color {
  width: $grid;
  height: $grid;
  //   margin-right: $grid;
  border: 1px solid #888;
}

.legend {
  position: absolute;
  bottom: calc(#{$gutter-width} * #{$grid});
  left: 0px;

  &.interactive {
    width: calc((#{$gutter-width} * 0 + #{$block-width} * 1) * #{$grid});
  }

  &.left {
    left: calc(-1 * (#{$gutter-width} + #{$block-width}) * #{$grid});
    bottom: 0px;
  }

  &.right {
    right: calc(-1 * (#{$gutter-width} + #{$block-width}) * #{$grid});
    left: unset;
    bottom: 0px;
  }

  &.below {
    position: absolute;
    top: calc(1 * (#{$gutter-width} * 1 + #{$block-width} * 2) * #{$grid});
  }

  .label {
    @include plumber(
      $grid-height: $grid,
      $baseline: 0.1,
      $font-size: 1,
      $line-height: 2,
      $leading-top: 0,
      $leading-bottom: 0
    );

    font-family: soleil;
    font-weight: 200;
  }

  .legend-row {
    display: flex;
  }

  .title {
    @include plumber(
      $grid-height: $grid,
      $baseline: 0.1,
      $font-size: 1,
      $line-height: 1,
      $leading-top: 0,
      $leading-bottom: 1
    );

    font-weight: bold;
    font-family: soleil;
  }
}

.container.h3 .legend.below {
  top: calc(1 * (#{$gutter-width} * 0 + #{$block-width} * 3) * #{$grid});
}

.ell {
  margin-left: 2px;
  margin-right: 2px;
  color: gray;
}

.menu {
  position: absolute;
  bottom: $grid;
  left: $grid;
  z-index: 10000;

  @media (max-width: 928px) {
    position: relative;

    .row {
      flex-wrap: wrap;

      .button {
        margin-bottom: $grid;
      }
    }
  }

  .row {
    display: flex;
    margin-bottom: $grid;
  }

  .button {
    box-sizing: border-box;
    background-color: white;
    border: 1px solid gray;
    margin-right: $grid;
    padding: 8px;
    cursor: pointer;
    opacity: 0.7;

    &:hover {
      opacity: 1;
    }

    &.active {
      background-color: rgb(157, 205, 224);
    }

    font-family: soleil;
    font-weight: 300;
    font-size: 12px;
  }
}

.zip {
  font-weight: bold;
}

.zip_row {
  display: flex;

  .title {
    margin-right: $grid;
    font-weight: 600;
  }
}

.info {
  height: calc(1 * (#{$gutter-width} * 2 + #{$block-width} * 1) * #{$grid});
  overflow-y: scroll;
}
</style>
