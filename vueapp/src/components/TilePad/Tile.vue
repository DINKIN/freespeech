<template>
  <v-card
    raised
    tile
    class="mx-auto"
    @click="tileClickedEvent"
    :color="typeof tileData.accent === 'undefined' ? '' : cardColor"
  >
    <v-container
      justify="
    center"
    >
      <v-row
        align="center"
        justify="center"
      >
        <v-img
          max-height="32"
          max-width="32"
          aspect-ratio="1"
          :src="tileData.image"
        />
      </v-row>
      <v-row>
        <v-card-text
          class
          align="center"
          justify="center"
        >
          <h3>{{ tileData.name }}</h3>
        </v-card-text>
      </v-row>
    </v-container>
  </v-card>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';

export default {
  name: 'Tile',
  props: {
    tileData: {
      type: Object,
      default: Object
    },
    tilePage:{
      type: String,
      default: ''
    }
  },
  computed: {
    ...mapGetters({
      editMode: 'tilePad/editMode'
    }),
    cardColor: function() {
      let cardHexColor;

      switch (this.tileData.accent) {
        case 'red':
          cardHexColor = '#FF9AA2';
          break;
        case 'blush':
          cardHexColor = '#FFB7B2';
          break;
        case 'peach':
          cardHexColor = '#FFB7B2';
          break;
        case 'pear':
          cardHexColor = '#E2F0CB';
          break;
        case 'mint':
          cardHexColor = '#B5EAD7';
          break;
        case 'violet':
          cardHexColor = '#C7CEEA';
          break;
        default:
          cardHexColor = '';
          break;
      }
      return cardHexColor;
    }
  },
  methods: {
    ...mapActions({
      toggleEditDialogVisibility: 'tilePad/toggleEditDialogVisibility',
      setCurrentTileBeingEdited: 'tilePad/setCurrentTileBeingEdited'
    }),
    tileClickedEvent() {
      if (this.editMode) {
        let tileDataToEdit = this.tileData;
        tileDataToEdit.page = this.tilePage;
        this.setCurrentTileBeingEdited(tileDataToEdit);
        this.toggleEditDialogVisibility();
      } else {
          if (typeof this.tileData.navigation === 'undefined' ||this.tileData.navigation === '') {
              this.$emit('speakText', this.tileData.text);
          } else {
              this.$router.push({
                name: 'tilePadWithRoute',
                params: { layout: this.tileData.navigation }
              });
            }
      }
    }
  }
};
</script>

<style></style>
