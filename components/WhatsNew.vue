<template>
  <div class="WhatsNew">
    <h2 class="WhatsNew-heading">
      <v-icon size="24" class="WhatsNew-heading-icon">
        mdi-information
      </v-icon>
      最新のお知らせ
    </h2>
    <ul class="WhatsNew-list">
      <li v-for="(item, i) in items" :key="i" class="WhatsNew-list-item">
        <a
          class="WhatsNew-list-item-anchor"
          :href="item.link"
          target="_blank"
          rel="noopener"
        >
          <time
            class="WhatsNew-list-item-anchor-time px-2"
            :datetime="item.date"
          >
            {{ item.date | convertDateToDisplayText }}
          </time>
          <span class="WhatsNew-list-item-anchor-link">
            {{ item.title }}
            <v-icon
              v-if="!isInternalLink(item.link)"
              class="WhatsNew-item-ExternalLinkIcon"
              size="12"
            >
              mdi-open-in-new
            </v-icon>
          </span>
        </a>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import Vue, { PropOptions } from 'vue'
import { convertDateToDisplayText } from '@/utils/formatDate'
import { NewsItem } from '@/tool/news-updater/types'

export default Vue.extend({
  filters: {
    convertDateToDisplayText
  },
  props: {
    items: {
      type: Array,
      required: true
    } as PropOptions<NewsItem[]>
  },
  methods: {
    isInternalLink(path: string) {
      return !/^https?:\/\//.test(path)
    }
  }
})
</script>

<style lang="scss">
.WhatsNew {
  @include card-container();
  padding: 10px;
  margin-bottom: 20px;
}

.WhatsNew-heading {
  display: flex;
  align-items: center;
  @include card-h2();
  margin-bottom: 12px;
  color: $gray-2;
  margin-left: 12px;

  &-icon {
    margin: 3px;
  }
}

.WhatsNew .WhatsNew-list {
  padding-left: 0px;
  list-style-type: none;

  &-item {
    &-anchor {
      display: flex;
      text-decoration: none;
      margin: 5px;
      font-size: 14px;

      @include lessThan($medium) {
        flex-wrap: wrap;
      }

      &-time {
        flex: 0 0 90px;
        @include lessThan($medium) {
          flex: 0 0 100%;
        }
        color: $gray-1;
      }

      &-link {
        flex: 0 1 auto;
        @include text-link();
        @include lessThan($medium) {
          padding-left: 8px;
        }
      }

      &-ExternalLinkIcon {
        margin-left: 2px;
        color: $gray-3 !important;
      }
    }
  }
}
</style>
