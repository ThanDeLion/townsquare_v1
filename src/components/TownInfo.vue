<template>

    <div class="box">
        <div></div>
        <table>
            <caption>
                <img class="edition_logo" style="width:400px" :src="`${
          edition.logo && grimoire.isImageOptIn
            ? edition.logo
            : require('../assets/editions/' + edition.id + '.png')
        }`
      ">
            </caption>
            <tr>
                <td colspan="4">
                </td>
            </tr>
            <tr>
                <td>Players</td>
                <td>{{ players.length }}</td>
                <td>Alive</td>
                <td>{{ teams.alive }}</td>
            </tr>
            <tr>
                <td class="townsfolk">Townsfolk</td>
                <td class="townsfolk">{{ teams.townsfolk }}</td>
                <td class="minion">Minions</td>
                <td class="minion">{{ teams.minion }}</td>
            </tr>
            <tr>
                <td class="outsider">Outsiders</td>
                <td class="outsider">{{ teams.outsider }}</td>
                <td class="demon">Demons</td>
                <td class="demon">{{ teams.demon }}</td>
            </tr>
            <tr v-if="teams.traveler" class="traveler">
                <td colspan="3">Travellers</td>
                <td>{{ teams.traveler }}</td>
            </tr>
        </table>
    </div>
</template>

<script>
import gameJSON from "./../game";
import { mapState } from "vuex";

export default {
  computed: {
    teams: function() {
      const { players } = this.$store.state.players;
      const nonTravelers = this.$store.getters["players/nonTravelers"];
      const alive = players.filter(player => player.isDead !== true).length;
      return {
        ...gameJSON[nonTravelers - 5],
        traveler: players.length - nonTravelers,
        alive,
        votes:
          alive +
          players.filter(
            player => player.isDead === true && player.isVoteless !== true
          ).length
      };
    },
    ...mapState(["edition", "grimoire"]),
    ...mapState("players", ["players"])
  }
};
</script>

<style lang="scss" scoped>
@import "../vars.scss";


    .box {
        position: absolute;
        border: 10px solid;
        -o-border-image: url("../assets/towninfo.122e2c26.webp") 40 fill/20px stretch;
        border-image: url("../assets/towninfo.122e2c26.webp") 40 fill/20px stretch;
        padding: 1vh calc(1vh + 10px) 1vh 1vh;
        font-size: 80%;
        color: #000;
        font-weight: 500;
        line-height: 1.1;
        display: flex;
        align-items: center;

        box-shadow: 0 0 10px rgba(0,0,0,.5) tr {
            white-space: nowrap
        }

        tr td:nth-child(odd) {
            text-align: center;
            padding: 0 10px
        }

        tr td:nth-child(2n) {
            text-align: center;
            font-size: 120%;
            font-weight: 500;
            border-left: 1px dotted #806151;
            border-right: 1px dotted #806151;
            padding: 0 10px
        }
    }

    .townsfolk {
        color: $townsfolk;
    }

    .outsider {
        color: $townsfolk;
    }

    .minion {
        color: $demon;
    }

    .demon {
        color: $demon;
    }

    .traveler {
        color: $traveler;
    }

</style>
