p<template>
  <div class="c-activeeventspopup">
    <button @click="toggle" class="toggle tour-event-popup">
      <span>Active Events</span>
      <font-awesome-icon
        v-if="!popupVisible"
        :icon="['fas', 'caret-up']"
        size="lg"
      />
      <font-awesome-icon
        v-if="popupVisible"
        :icon="['fas', 'caret-down']"
        size="lg"
      />
    </button>
    <div class="wrapper" :style="position">
      <p class="empty" v-if="eventsForTheRound.length === 0">
        No Active Events
      </p>
      <EventCard
        v-for="(event, index) in eventsForTheRound"
        :key="index"
        :event="event"
        :visible="eventVisible(index)"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop, Inject } from 'vue-property-decorator';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faCaretUp } from '@fortawesome/free-solid-svg-icons/faCaretUp';
import { faCaretDown } from '@fortawesome/free-solid-svg-icons/faCaretDown';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import EventCard from '@port-of-mars/client/components/game/phases/events/EventCard.vue';
import { Phase } from '@port-of-mars/shared/types';
import { GameRequestAPI } from '@port-of-mars/client/api/game/request';

library.add(faCaretUp);
library.add(faCaretDown);
Vue.component('font-awesome-icon', FontAwesomeIcon);

@Component({
  components: {
    EventCard,
  },
})
export default class ActiveEventsPopup extends Vue {
  @Inject()
  readonly api!: GameRequestAPI;

  get popupVisible() {
    return this.$tstore.state.userInterface.popupView.activeEventsVisible;
  }

  private toggle() {
    this.api.toggleActiveEvents(this.popupVisible);
  }

  get position() {
    return this.popupVisible ? { height: '48rem',padding: '0.5rem' } : { height: '0rem' };
  }

  get eventsForTheRound() {
    return this.$tstore.state.marsEvents;
  }

  get eventsProcessed(): number {
    return this.$tstore.state.marsEventsProcessed;
  }

  private eventVisible(index: number) {
    return (
      this.$tstore.state.phase !== Phase.events || index <= this.eventsProcessed
    );
  }
}
</script>

<style lang="scss" scoped>
@import '@port-of-mars/client/stylesheets/game/static/popups/ActiveEventsPopup.scss';
</style>
