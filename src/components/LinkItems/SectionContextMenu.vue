<template>
  <transition name="slide">
    <div class="context-menu" v-if="show && !isMenuDisabled"
      :style="posX && posY ? `top:${posY}px;left:${posX}px;` : ''">
      <!-- Open Options -->
      <ul class="menu-section">
        <li @click="openSection()">
          <SameTabOpenIcon />
          <span>{{ $t('context-menus.section.open-section') }}</span>
        </li>
        <li @click="openEditSectionMenu">
          <EditIcon />
          <span>{{ $t('context-menus.section.edit-section') }}</span>
        </li>
        <li v-if="isEditMode" @click="removeSection">
          <BinIcon />
          <span>{{ $t('context-menus.section.remove-section') }}</span>
        </li>
      </ul>
    </div>
  </transition>
</template>

<script>
// Import icons for each element
import EditIcon from '@/assets/interface-icons/config-edit-json.svg';
import BinIcon from '@/assets/interface-icons/interactive-editor-remove.svg';
import SameTabOpenIcon from '@/assets/interface-icons/open-current-tab.svg';

export default {
  name: 'ContextMenu',
  components: {
    EditIcon,
    BinIcon,
    SameTabOpenIcon,
  },
  props: {
    posX: Number, // The X coordinate for positioning
    posY: Number, // The Y coordinate for positioning
    show: Boolean, // Should show or hide the menu
  },
  computed: {
    isMenuDisabled() {
      return !!this.$store.getters.appConfig.disableContextMenu;
    },
    isEditMode() {
      return this.$store.state.editMode;
    },
  },
  methods: {
    /* Called on item click, emits an event up to Item */
    /* in order to launch the current app to a given target */
    openSection() {
      this.$emit('navigateToSection');
    },
    openEditSectionMenu() {
      this.$emit('openEditSection');
    },
    removeSection() {
      this.$emit('removeSection');
    },
  },
};
</script>

<style scoped lang="scss">

div.context-menu {
  position: absolute;
  margin: 0;
  padding: 0;
  z-index: 8;
  background: var(--context-menu-background);
  color: var(--context-menu-color);
  border: 1px solid var(--context-menu-secondary-color);
  border-radius: var(--curve-factor);
  box-shadow: var(--context-menu-shadow);
  opacity: 0.98;

  ul.menu-section {
    list-style-type: none;
    margin: 0;
    padding: 0;
    &:not(:last-child) {
      border-bottom: 1px solid var(--context-menu-color);
    }
    li {
      cursor: pointer;
      padding: 0.5rem 1rem;
      display: flex;
      flex-direction: row;
      font-size: 1rem;
      &:not(:last-child) {
        border-bottom: 1px solid var(--context-menu-secondary-color);
      }
      svg {
        width: 1rem;
         margin-right: 0.5rem;
          path { fill: currentColor; }
      }
    }
  }
}

// Define enter and leave transitions
.slide-enter-active { animation: slide-in .1s; }
.slide-leave-active { animation: slide-in .1s reverse; }
@keyframes slide-in {
  0% { transform: scaleY(0.5) scaleX(0.8) translateY(-50px); }
  100% { transform: scaleY(1) translateY(0) translateY(0); }
}
</style>
