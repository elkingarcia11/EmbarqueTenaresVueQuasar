<template>
  <q-page-container id="home-body">
    <q-form @submit="submit" class="invoiceForm">
      <q-input
        ref="invoiceInputRef"
        square
        class="window-width overflow-hidden bg-white"
        outlined
        v-model="invoiceText"
        :label="$t('trackPack')"
        mask="############"
        unmasked-value
      >
        <template v-slot:append>
          <q-btn
            flat
            round
            color="secondary"
            icon="quiz"
            @click="invoiceDialog = true"
          />
          <q-btn flat round color="primary" icon="search" @click="submit" />
        </template>
      </q-input>
    </q-form>
    <div id="logoDiv">
      <q-img id="logo" src="../assets/logo.png" fit="contain" />
    </div>

    <TabBar
      v-if="$q.platform.is.mobile"
      ref="tabBarRef"
      :buttonNumber="0"
      @focus-input="$refs['invoiceInputRef'].focus()"
    />
    <q-dialog v-model="invoiceDialog" transition-hide="slide-down">
      <q-card>
        <q-toolbar class="bg-primary">
          <div class="text-white text-center q-px-sm q-py-md dialogToolbar">
            {{ $t('findInv')
            }}<span class="text-weight-bold">{{ $t('findInvTwo') }}</span>
          </div>
        </q-toolbar>
        <q-separator />
        <q-card-section class="row full-height justify-center">
          <img
            loading="lazy"
            v-if="$i18n.locale == 'en-US'"
            class="self-center"
            id="logo"
            fit="contain"
            src="../assets/trackEN.png"
          />
          <img
            loading="lazy"
            v-else
            class="self-center"
            id="logo"
            fit="contain"
            src="../assets/trackES.png"
          />
        </q-card-section>
      </q-card>
    </q-dialog>
    <FooterComponent />
  </q-page-container>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import TabBar from 'src/components/TabBar.vue';
import FooterComponent from 'src/components/FooterComponent.vue';
import '../css/home.scss';

export default defineComponent({
  components: {
    TabBar,
    FooterComponent,
  },
  setup() {
    return {
      invoiceDialog: ref(false),
      invoiceText: '',
      successfullyRetrieved: false,
    };
  },
  methods: {
    submit() {
      // Submit
      if (this.invoiceText === '') {
        (this.$refs['invoiceInputRef'] as any).focus();
      } else {
        (this.$refs['invoiceInputRef'] as any).blur();
        this.$router.push({
          name: 'search',
          params: { invoice: this.invoiceText },
        });
      }
    },
  },
});
</script>
