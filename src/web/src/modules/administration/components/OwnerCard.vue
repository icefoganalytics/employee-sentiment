<template>
  <v-card elevation="3" color="#F2A90066" style="height: 150px" @click="goToModeration">
    <v-card-text style="text-align: right">
      <v-icon class="float-left" style="font-size: 90px; opacity: 25%; position: absolute; left: 0px; margin-top: 30px"
        >mdi-lightbulb</v-icon
      >

      <div style="height: 52px">
        <div style="height: 50px; font-size: 16px; line-height: 21px">
          <div>{{ question.TITLE }}</div>
          <v-divider class="mb-1"></v-divider>
          <span style="font-size: 13px"
            >{{ participantStats.opinionators }} Opinionators, {{ participantStats.raters }} Raters -
          </span>
          <span style="font-weight: 700">{{ stateTitle(question.STATE) }}</span>
        </div>

        <div class="float-right text-center ml-10">
          <span style="font-size: 40px">{{ ratingCountForQuestion(question.ID) }}</span
          ><br />
          Total Ratings
        </div>
        <div class="float-right text-center ml-10">
          <span style="font-size: 40px">{{ moderatedCountForQuestion(question.ID) }}</span
          ><br />
          Moderated
        </div>
        <div class="float-right text-center">
          <span style="font-size: 40px">{{ unmoderatedCountForQuestion(question.ID) }}</span
          ><br />
          Unmoderated
        </div>
      </div>
    </v-card-text>
  </v-card>
</template>

<script lang="ts">
import { useQuestionStore } from "@/modules/administration/modules/question/store";
import { mapActions } from "pinia";
import { useResponseStore } from "../modules/response/store";
import { useParticipantsStore } from "../modules/participants/store";

export default {
  name: "ModeratorCard",
  props: ["question"],
  data: () => ({
    participantStats: { raters: 0, opinionators: 0 },
  }),
  computed: {},
  async mounted() {
    this.participantStats = await this.getParticipantStats(this.question.ID);
  },
  methods: {
    ...mapActions(useQuestionStore, ["stateTitle"]),
    ...mapActions(useParticipantsStore, ["getParticipantStats"]),
    ...mapActions(useResponseStore, [
      "moderatedCountForQuestion",
      "unmoderatedCountForQuestion",
      "ratingCountForQuestion",
    ]),

    goToModeration() {
      this.$router.push(`/administration/moderation/${this.question.ID}`);
    },
  },
};
</script>
