<template>

  <HorizontalNavBarWithOverflowMenu
    v-if="classId"
    :navigationLinks="links"
  />

</template>


<script>

  import { mapState } from 'vuex';
  import HorizontalNavBarWithOverflowMenu from 'kolibri.coreVue.components.HorizontalNavBarWithOverflowMenu';
  import commonCoreStrings from 'kolibri.coreVue.mixins.commonCoreStrings';
  import { PageNames } from '../constants';
  import { coachStringsMixin } from './common/commonCoachStrings';

  export default {
    name: 'TopNavbar',
    components: {
      HorizontalNavBarWithOverflowMenu,
    },
    mixins: [coachStringsMixin, commonCoreStrings],

    computed: {
      ...mapState('classSummary', { classId: 'id' }),
      links() {
        return [
          {
            title: this.coreString('classHome'),
            link: this.navRoute('HomePage'),
            icon: 'dashboard',
            color: this.$themeTokens.textInverted,
          },
          {
            title: this.coachString('reportsLabel'),
            link: this.navRoute(PageNames.REPORTS_PAGE),
            icon: 'reports',
            color: this.$themeTokens.textInverted,
          },
          {
            title: this.$tr('plan'),
            link: this.navRoute(PageNames.PLAN_PAGE),
            icon: 'edit',
            color: this.$themeTokens.textInverted,
          },
        ];
      },
    },
    methods: {
      navRoute(name) {
        return this.classId ? { name, params: { classId: this.classId } } : { name };
      },
    },
    $trs: {
      plan: {
        message: 'Plan',
        context:
          "Translate as a VERB. Refers to the 'Plan' tab where coaches manage lessons, quizzes, and groups.",
      },
    },
  };

</script>


<style lang="scss" scoped></style>
