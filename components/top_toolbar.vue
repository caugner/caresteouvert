<template>
  <div class="search ml-md-5 mt-md-5">
    <v-toolbar
      :dense="!isMobile || !geocoder"
      :height="isMobile && geocoder ? '70px' : undefined"
      v-resize="resize"
    >
      <v-tooltip
        v-if="!isMobile"
        bottom
      >
        <template v-slot:activator="{ on }">
          <v-btn
            icon
            v-on="on"
            @click.native="$emit('toggleSidebar')"
          >
            <v-icon>osm-filter_list</v-icon>
          </v-btn>
        </template>
        <span>{{ $t('menu') }}</span>
      </v-tooltip>

      <template v-if="!geocoder">
        <img
          :alt="$t('subtitle-dense')"
          :src="logoMobile"
          class="img-header-mobile"
        />
        <v-spacer></v-spacer>
      </template>
      <v-tooltip
        v-if="!geocoder"
        bottom
      >
        <template v-slot:activator="{ on }">
          <v-btn
            icon
            v-on="on"
            @click="geocoder = true"
          >
            <v-icon>osm-magnify</v-icon>
          </v-btn>
        </template>
        <span>{{ $t('search') }}</span>
      </v-tooltip>

      <geocoder
        v-show="geocoder"
        @select="onGeocoderSelect"
        @blur="onGeocoderBlur"
      />
    </v-toolbar>
  </div>
</template>

<script>
import Geocoder from './geocoder';
import i18nMixin from './mixins/i18n';

export default {
  components: {
    Geocoder
  },

  mixins: [i18nMixin],

  data() {
    return {
      geocoder: false,
      isMobile: false
    };
  },

  mounted() {
    this.resize();
    this.geocoder = !this.isMobile;
  },

  methods: {
    resize() {
      this.isMobile = this.$vuetify.breakpoint.smAndDown;
    },

    onGeocoderSelect(bbox) {
      this.geocoder = !this.isMobile;
      this.$emit('onGeocode', bbox);
    },

    onGeocoderBlur() {
      this.geocoder = !this.isMobile;
    }
  }
}
</script>

<style>
.search {
  position: absolute;
  z-index: 4;
}
.sm .search {
  width: 100%;
}
.img-header-mobile {
  max-height: 40px;
  max-width: 80%;
}
</style>
