<template>
  <!-- Section containing the list of FAQs -->
  <q-item-section
    class="q-py-lg text-h4 text-bold text-center"
    style="font-family: 'BodoniSvtyTwoSCITCTT-Book'"
  >
    <!-- Title of the FAQs section -->
    {{ $t('faqs') }}
  </q-item-section>
  <div>
    <!-- Iterate through the list of FAQs -->
    <q-expansion-item
      v-for="faq in faqsList"
      :key="faq.id"
      expand-separator
      expanded-icon="arrow_drop_down"
      expand-icon="arrow_right"
      :label="printQuestion(faq)"
      header-class="q-py-lg bg-white text-black faqsExpansionHeaderStyle"
      expand-icon-class="text-primary"
    >
      <q-card>
        <!-- Display the FAQ answer based on the selected locale -->
        <q-card-section v-if="$i18n.locale == 'en-US'" style="font-size: 20px">
          {{ faq.a_en }}
        </q-card-section>
        <q-card-section v-else style="font-size: 18px" class="answers q-pl-xl">
          {{ faq.a_es }}
        </q-card-section>
      </q-card>
    </q-expansion-item>
  </div>
</template>

<script lang="ts">
import { computed } from 'vue';
import { onBeforeMount } from 'vue';
import { useI18n } from 'vue-i18n';
import { DocumentData } from 'firebase/firestore';
import { useQuasar } from 'quasar';

import '../css/faqs.scss';
import { useStore } from 'src/store';

export default {
  setup() {
    // Access Quasar utilities
    const $q = useQuasar();
    const $store = useStore();
    const { locale } = useI18n();

    // Compute the list of FAQs from the store
    const faqsList = computed(() => $store.state.faqs.faqsList);

    // Function to print the question based on the selected locale
    const printQuestion = (question: DocumentData) => {
      return locale.value == 'en-US' ? question.q_en : question.q_es;
    };

    // Fetch FAQs before component mount
    onBeforeMount(async () => {
      $q.loading.show();
      if (faqsList.value.length < 1) {
        await $store.dispatch('faqs/fetchFaqs');
      }
      $q.loading.hide();
    });

    return {
      faqsList,
      printQuestion,
    };
  },
};
</script>
