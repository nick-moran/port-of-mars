<template>
  <div @click="handleOpenModal()" class="c-player">
    <div class="indicator" :style="indicatorStyle">
      <div class="frame" :style="frameColor">
        <img :src="playerRoleImage" alt="Player Image" />
      </div>
    </div>
    <div class="information">
      <p class="title">Your Role</p>
      <p class="role">{{ playerRole }}</p>
      <p class="score">Score: {{ playerScore }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Inject, Prop } from 'vue-property-decorator';
import { Role } from '@port-of-mars/shared/types';
import { TutorialAPI } from '@port-of-mars/client/api/tutorial/request';
@Component({
  components: {},
})

export default class Player extends Vue {
  @Inject()
  readonly api!: TutorialAPI;

  get playerRole(): Role {
    return this.$tstore.state.role;
  }

  get playerScore(): number {
    return this.playerRole ? this.$tstore.state.players[this.playerRole].victoryPoints : 0;
  }

  get playerReady() {
    return this.$store.getters.player.ready;
  }

  get frameColor(): object {
    return this.playerRole
      ? { backgroundColor: `var(--color-${this.playerRole})` }
      : { backgroundColor: `var(--color-Researcher)` };
  }

  get indicatorStyle() {
    return !this.playerReady
      ? { border: `0.25rem solid var(--color-${this.playerRole})` }
      : { border: `0.25rem solid var(--status-green)` };
  }

  get playerRoleImage(): any {
    return this.playerRole
      ? require(`@port-of-mars/client/assets/characters/${this.playerRole}.png`)
      : require(`@port-of-mars/client/assets/characters/Researcher.png`);
  }

  handleOpenModal() {
    let data = {
      type: 'PlayerModal',
      data: {
        role: this.playerRole,
      },
    }
    this.api.setModalVisible(data)
  }
}
</script>

<style lang="scss" scoped>
@import "@port-of-mars/client/stylesheets/game/static/panels/Player.scss";
</style>
