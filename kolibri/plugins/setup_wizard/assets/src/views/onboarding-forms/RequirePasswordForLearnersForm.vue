<template>

  <OnboardingStepBase
    :title="$tr('header')"
    :eventOnGoBack="backEvent"
    @continue="handleContinue"
  >
    <KRadioButton
      ref="yesRadio"
      v-model="setting"
      class="radio-button"
      :label="$tr('yesOptionLabel')"
      :value="true"
    />
    <KRadioButton
      ref="noRadio"
      v-model="setting"
      class="radio-button"
      :label="$tr('noOptionLabel')"
      :value="false"
    />
    <p class="description">
      {{ $tr('changeLater') }}
    </p>
  </OnboardingStepBase>

</template>


<script>

  import { Presets } from '../../constants';
  import OnboardingStepBase from '../OnboardingStepBase';

  export default {
    name: 'RequirePasswordForLearnersForm',
    components: {
      OnboardingStepBase,
    },
    inject: ['wizardService'],
    data() {
      let setting = this.wizardService.state.context['requirePassword'];
      if (setting === null) {
        // Set default for the setting if one isn't selected; depends on the preset selected
        const preset = this.wizardService.state.context['formalOrNonformal'];
        setting = preset === Presets.NONFORMAL;
      }
      return {
        setting,
      };
    },
    computed: {
      backEvent() {
        return { type: 'BACK', value: Boolean(this.setting) };
      },
    },
    methods: {
      handleContinue() {
        this.wizardService.send({ type: 'CONTINUE', value: this.setting });
      },
    },
    $trs: {
      header: {
        message: 'Enable passwords on learner accounts?',
        context:
          'Admins can either enable passwords for learners or offer a simplified sign-in, without the password requirement. This allows easier access for younger learners.',
      },
      yesOptionLabel: {
        message: 'Yes',
        context:
          "Option on the 'Enable passwords for learners' screen. The admin selects this option if they don't want to enable passwords for learner accounts.",
      },
      noOptionLabel: {
        message: 'No. Learner accounts can sign in with just a username',
        context:
          "Option on the 'Enable passwords for learners' screen. The admin selects this option if they don't want to enable passwords for learner accounts.",
      },
      changeLater: {
        message: 'You can change this in your learning facility settings later',
        context: '',
      },
    },
  };

</script>


<style lang="scss" scoped>

  .title {
    font-size: 1.5em;
  }

  .description {
    padding-bottom: 8px;
    font-size: 0.875em;
  }

  .radio-button {
    padding-bottom: 8px;
    font-size: 0.875em;
  }

</style>
