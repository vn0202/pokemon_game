<template>
  <div class="interact-screen" style="height: 100vh">
    <card-item
      v-for="(card, idx) in cardsContent"
      :key="idx"
      :ref="`card-${idx}`"
      :img-back="`images/${card}`"
      :card="{ index: idx, value: card }"
      :rate="rate"
      @onFlip="checkRule($event)"
    ></card-item>
  </div>
</template>
<script>
import CardItem from "@/components/CardItem.vue";

export default {
  props: {
    cardsContent: {
      type: Array,
      default: () => [],
    },
  },
  components: {
    CardItem,
  },
  data() {
    return {
      rule: [],
      machine_cacular: 0,
      time: new Date().getTime(),
      rate: Math.sqrt(this.cardsContent.length),
    };
  },
  methods: {
    checkRule(card) {
      if (this.rule.length === 2) {
        return false;
      }
      this.rule.push(card);
      let that = this;
      if (
        this.rule.length === 2 &&
        this.rule[0].value === this.rule[1].value &&
        this.rule[0].index !== this.rule[1].index
      ) {
        this.$refs[`card-${this.rule[0].index}`][0].disabled = true;
        this.$refs[`card-${this.rule[1].index}`][0].disabled = true;
        let rule_tmp = that.rule;
        setTimeout(function () {
          that.$refs[`card-${rule_tmp[0].index}`][0].is_show = false;
          that.$refs[`card-${rule_tmp[1].index}`][0].is_show = false;
        }, 800);
        this.rule = [];
        this.machine_cacular++;
        if (this.machine_cacular === this.cardsContent.length / 2) {
          setTimeout(() => {
            let result = new Date().getTime() - this.time;
            this.$emit("onSuccess", result);
          }, 800);
        }
      } else if (
        (this.rule.length === 2 && this.rule[0].value !== this.rule[1].value) ||
        this.rule[0].index === this.rule[1].index
      ) {
        setTimeout(() => {
          this.$refs[`card-${this.rule[0].index}`][0].onCloseFlip();
          this.$refs[`card-${this.rule[1].index}`][0].onCloseFlip();
          this.rule = [];
        }, 800);
      } else {
        return false;
      }
    },
  },
};
</script>
<style lang="css" scoped>
.interact-screen {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  align-content: baseline;
}
@media (min-width: 960px) {
  .interact-screen {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    align-content: baseline;
  }
}
</style>
