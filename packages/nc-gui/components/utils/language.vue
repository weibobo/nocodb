<template>
  <div>
    <v-menu bottom offset-y>
      <template #activator="{on}">
        <v-icon size="20" class="ml-2" v-on="on">
          mdi-translate
        </v-icon>
      </template>
      <v-list dense>
        <v-list-item-group
          v-model="language"
        >
          <v-list-item
            v-for="lan in languages"
            :key="lan.value"
            dense
            :value="lan"
            color="primary"
            @click="changeLan(lan)"
          >
            <v-list-item-subtitle class="text-capitalize">
              {{ labels[lan] || lan }}
            </v-list-item-subtitle>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-menu>
  </div>
</template>

<script>
const order = l => ({ zh_CN: 10, en: 11, id: -2 })[l] || 0
export default {
  name: 'Language',
  data: () => ({
    labels: {
      de: 'Deutsch',
      en: 'English',
      es: 'Española',
      fr: 'français',
      id: 'bahasa Indonesia',
      it_IT: 'Italiana',
      ja: '日本語',
      ko: '한국인',
      nl: 'Nederlandse',
      ru: 'русский',
      zh_CN: '中国人',
      zh_HK: 'zh_HK',
      zh_TW: 'zh_TW'
    }
  }),
  computed: {
    languages() {
      return ((this.$i18n && this.$i18n.availableLocales) || ['en']).sort((a, b) => order(b) - order(a))
    },
    language: {
      get() {
        return this.$store.state.windows.language
      },
      set(val) {
        this.$store.commit('windows/MutLanguage', val)
      }
    }
  },
  methods: {
    changeLan(lan) {
      this.language = lan
      const count = 200
      const defaults = {
        origin: { y: 0.7 }
      }

      function fire(particleRatio, opts) {
        window.confetti(Object.assign({}, defaults, opts, {
          particleCount: Math.floor(count * particleRatio)
        }))
      }

      fire(0.25, {
        spread: 26,
        startVelocity: 55
      })
      fire(0.2, {
        spread: 60
      })
      fire(0.35, {
        spread: 100,
        decay: 0.91,
        scalar: 0.8
      })
      fire(0.1, {
        spread: 120,
        startVelocity: 25,
        decay: 0.92,
        scalar: 1.2
      })
      fire(0.1, {
        spread: 120,
        startVelocity: 45
      })
    }
  }
}
</script>

<style scoped>

</style>
