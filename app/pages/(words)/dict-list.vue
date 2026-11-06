<script setup lang="ts">
import { _nextTick, groupBy, isMobile, loadJsLib, resourceWrap, useNav } from '@/core/utils'
import { BackIcon, BaseButton, BaseIcon, BaseInput, BasePage } from '@/base'
import type { DictResource } from '@/core/types/types.ts'
import { useRuntimeStore } from '@/core/stores/runtime.ts'
import Empty from '@/components/Empty.vue'
import DictList from '@/components/list/DictList.vue'
import DictGroup from '@/components/list/DictGroup.vue'
import { useBaseStore } from '@/core/stores/base.ts'
import { useRouter } from 'vue-router'
import { computed, watch } from 'vue'
import { getDefaultDict } from '@/core/types/func.ts'
import { useFetch } from '@vueuse/core'
import { DICT_LIST, LIB_JS_URL, TourConfig } from '@/core/config/env.ts'
import { withAppBaseURL } from '@/core/utils/base-url'
import { useSettingStore } from '@/core/stores/setting.ts'

const { nav } = useNav()
const runtimeStore = useRuntimeStore()
const settingStore = useSettingStore()
const store = useBaseStore()
const router = useRouter()

function selectDict(e) {
  console.log(e.dict)
  getDictDetail(e.dict)
}

async function getDictDetail(val: DictResource) {
  runtimeStore.editDict = getDefaultDict(val)
  nav('/dict', { from: 'list' })
}

function groupByDictTags(dictList: DictResource[]) {
  return dictList.reduce<Record<string, DictResource[]>>((result, dict) => {
    dict.tags.forEach(tag => {
      if (result[tag]) {
        result[tag].push(dict)
      } else {
        result[tag] = [dict]
      }
    })
    return result
  }, {})
}

const { data: dict_list, isFetching } = useFetch(resourceWrap(DICT_LIST.WORD.ALL)).json()

const groupedByCategoryAndTag = $computed(() => {
  let data = []
  if (!dict_list.value) return data
  const groupByCategory = groupBy(dict_list.value, 'category')
  for (const [key, value] of Object.entries(groupByCategory)) {
    data.push([key, groupByDictTags(value)])
  }
  // ;[data[2], data[3]] = [data[3], data[2]]
  // console.log('data', data)
  return data
})

let showSearchInput = $ref(false)
let searchKey = $ref('')

const searchList = computed<any[]>(() => {
  if (searchKey) {
    let s = searchKey.toLowerCase()
    return dict_list.value.filter(item => {
      return (
        item.enName.toLowerCase().includes(s) ||
        item.name.toLowerCase().includes(s) ||
        item.category.toLowerCase().includes(s) ||
        item.tags.join('').replace('所有', '').toLowerCase().includes(s) ||
        item?.url?.toLowerCase?.().includes?.(s)
      )
    })
  }
  return []
})

watch(dict_list, val => {
  const recommendedDict = val?.find(dict => dict.recommended)
  if (!recommendedDict) return
  _nextTick(async () => {
    const Shepherd = await loadJsLib('Shepherd', LIB_JS_URL.SHEPHERD)
    const tour = new Shepherd.Tour(TourConfig)
    tour.on('cancel', () => {
      localStorage.setItem('tour-guide', '1')
    })
    tour.addStep({
      id: 'step2',
      text: '选一本自己准备学习的词典',
      attachTo: { element: `#dict-${recommendedDict.id}`, on: 'bottom' },
      buttons: [
        {
          text: `下一步（2/${TourConfig.total}）`,
          action() {
            tour.next()
            selectDict({ dict: recommendedDict })
          },
        },
      ],
    })

    const r = localStorage.getItem('tour-guide')
    if (settingStore.first && !r && !isMobile()) {
      tour.start()
    }
  }, 500)
})
</script>

<template>
  <BasePage>
    <div class="card min-h-200 dict-list-page" v-loading="isFetching">
      <div class="flex items-center relative gap-2 header-section">
        <BackIcon class="z-2" @click="router.back" />
        <div class="flex flex-1 gap-4" v-if="showSearchInput">
          <BaseInput clearable placeholder="请输入词典名称/缩写/类别" v-model="searchKey" class="flex-1" autofocus />
          <BaseButton @click="((showSearchInput = false), (searchKey = ''))">{{ $t('cancel') }}</BaseButton>
        </div>
        <div class="py-1 flex flex-1 justify-end" v-else>
          <span class="page-title absolute w-full center">{{ $t('dict_list') }}</span>
          <BaseIcon :title="$t('search')" @click="showSearchInput = true" class="z-1" icon="fluent:search-24-regular">
            <IconFluentSearch24Regular />
          </BaseIcon>
        </div>
      </div>
      <details v-if="!searchKey" class="my-4 rounded-lg border border-[var(--color-border)] p-4 text-sm leading-6">
        <summary class="cursor-pointer font-semibold">学习指南 · 从零基础到座舱 TPM</summary>
        <p class="mt-3">27 本词库、4,774 条练习。按编号从 01 开始。已有日常基础时，可以从职场沟通切入，把通用阅读词库当作并行补充。</p>
        <ol class="my-3 list-decimal ps-6">
          <li><strong>基础入门（445 条）</strong>。用简单句做自我介绍，能问时间、地点和价格。</li>
          <li><strong>生活会话（990 条）</strong>。练一次预约、问路、购物或改行程，能表达需求并复述关键细节。</li>
          <li><strong>通用阅读拓展（1,647 条）</strong>。读一段简短英语，圈出陌生词，试着用一句话复述主旨。可以和职场阶段并行。</li>
          <li><strong>职场沟通（910 条）</strong>。写一封简短确认邮件，把事项、负责人和时间说清楚。</li>
          <li><strong>TPM 项目与交付（412 条）</strong>。用英文汇报里程碑、风险、依赖和请求决策的事项。</li>
          <li><strong>座舱技术与实战（370 条）</strong>。说明故障现象、影响、证据、当前假设、责任人和关闭条件。</li>
        </ol>
        <p>每天先复习错词和到期词，再练一小组新词。先跟打并读出例句，然后遮住英文试着说或默写。每个阶段做一次上面的输出任务，隔天再试一遍。</p>
        <p class="mt-2"><strong>职场练习</strong>。Could you confirm the owner and the due date? 请确认负责人和截止日期。</p>
        <p class="mt-2"><strong>座舱汇报练习</strong>。We reproduced the screen freeze after wake-up. The root cause is still under investigation. 我们复现了唤醒后的屏幕冻结，根因仍在排查中。</p>
        <p class="mt-2">场景词库提供中文释义和双语例句。通用与商务词汇册按词频编排，提供释义、可用的音标和词形。<a :href="withAppBaseURL('/word-sources/NOTICE.txt')" class="underline" target="_blank" rel="noopener">词表来源与授权</a></p>
      </details>
      <div class="mt-4" v-if="searchKey">
        <DictList
          v-if="searchList.length"
          @selectDict="selectDict"
          :list="searchList"
          quantifier="词"
          :select-id="'-1'"
        />
        <Empty v-else text="没有相关词典" />
      </div>
      <div class="w-full" v-else>
        <DictGroup
          v-for="item in groupedByCategoryAndTag"
          :select-id="store.sdict.id"
          @selectDict="selectDict"
          quantifier="词"
          :groupByTag="item[1]"
          :category="item[0]"
        />
      </div>
    </div>
  </BasePage>
</template>

<style scoped lang="scss">

@media (max-width: 768px) {
  .dict-list-page {
    padding: 0.8rem;
    margin-bottom: 1rem;

    .header-section {
      flex-direction: column;
      gap: 0.5rem;

      .flex.flex-1.gap-4 {
        width: 100%;

        .base-input {
          font-size: 0.9rem;
        }

        .base-button {
          padding: 0.5rem 0.8rem;
          font-size: 0.9rem;
        }
      }

      .py-1.flex.flex-1.justify-end {
        width: 100%;

        .page-title {
          font-size: 1.2rem;
        }

        .base-icon {
          font-size: 1.2rem;
        }
      }
    }

    .mt-4 {
      margin-top: 0.8rem;
    }
  }
}

// 超小屏幕适配
@media (max-width: 480px) {
  .dict-list-page {
    padding: 0.5rem;

    .header-section {
      .flex.flex-1.gap-4 {
        .base-input {
          font-size: 0.8rem;
        }

        .base-button {
          padding: 0.4rem 0.6rem;
          font-size: 0.8rem;
        }
      }

      .py-1.flex.flex-1.justify-end {
        .page-title {
          font-size: 1rem;
        }

        .base-icon {
          font-size: 1rem;
        }
      }
    }
  }
}
</style>
