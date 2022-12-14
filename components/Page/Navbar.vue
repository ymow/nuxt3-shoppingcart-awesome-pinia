<script lang="ts" setup>
import { IApp } from '~/utils/app'
import { userCart } from '@/stores/newcart'
const cart = userCart()
// const showHistory = ref([])
const showCartItems = ref(false)
const showMenu = ref(false)

export interface IMenuItem {
  type: 'link' | 'button'
  text: string
  href?: any
  route?: any
}

const { t } = useLang()
const app = useState<IApp>('app')
const menus = computed((): IMenuItem[] => [
  {
    type: 'link',
    text: t('pages.getting-started.nav'),
    route: { name: 'getting-started' },
  },
  { type: 'link', text: t('pages.blank.nav'), route: { name: 'blank' } },
  { type: 'link', text: t('pages.test.nav'), route: { name: 'test' } },
  { type: 'link', text: t('pages.post.nav'), route: { name: 'post' } },
  { type: 'link', text: t('pages.setting.nav'), route: { name: 'setting' } },
  {
    type: 'button',
    text: t('pages.dashboard.nav'),
    route: { name: 'dashboard' },
  },
  {
    type: 'button',
    text: t('pages.cart.nav'),
    route: { name: 'cart' },
  },
])
</script>

<template>
  <BuilderNavbar>
    <template #banner>
      <div
        class="text-white text-xs text-center py-1 px-4 lg:px-8 bg-primary-500 capitalize"
      >
        <span class="mr-1">
          {{ $t('banners.welcome', { app_name: app.name }) }}
        </span>
        <Anchor
          class="underline font-bold"
          :text="$t('others.learn_more')"
          href="https://github.com/viandwi24/nuxt3-awesome-starter"
        />
      </div>
    </template>
    <template #menu>
      <div class="relative hidden lg:flex items-center ml-auto">
        <nav
          class="text-sm leading-6 font-semibold text-gray-600 dark:text-gray-300"
          role="navigation"
        >
          <ul class="flex items-center space-x-8">
            <li v-for="(item, i) in menus" :key="i">
              <Anchor
                v-if="item.type === 'link'"
                :to="item.route ? item.route : undefined"
                :href="item.href ? item.href : undefined"
                class="hover:no-underline hover:text-slate-900 hover:dark:text-white capitalize"
                >{{ item.text }}</Anchor
              >
              <Button
                v-else-if="item.type === 'button'"
                :text="item.text"
                size="xs"
                class="font-extrabold capitalize"
                :to="item.route ? item.route : undefined"
                :href="item.href ? item.href : undefined"
              />
            </li>
            <div class="flex justify-start items-start">
              <div class="relative">
                <NuxtLink
                  to="/cart"
                  class="flex flex-row cursor-pointer truncate p-2 px-4 rounded"
                  @blur="showCartItems = false"
                  @mouseover="showCartItems = !showCartItems"
                >
                  <div class="flex flex-row-reverse ml-2 w-full">
                    <div class="relative">
                      <div
                        class="absolute text-xs rounded-full -mt-1 -mr-2 px-1 font-bold top-0 right-0 bg-red-700 text-white"
                      >
                        {{ cart.cartItems.length }}
                      </div>
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="100%"
                        height="100%"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                        stroke-width="2"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        class="feather feather-shopping-cart w-6 h-6 mt-2"
                      >
                        <circle cx="9" cy="21" r="1"></circle>
                        <circle cx="20" cy="21" r="1"></circle>
                        <path
                          d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"
                        ></path>
                      </svg>
                    </div>
                  </div>
                </NuxtLink>
                <div
                  v-if="showCartItems"
                  class="absolute w-full rounded-b border-t-0 z-99"
                >
                  <div class="shadow-xl w-64">
                    <div
                      v-for="item in cart.cartItems"
                      :key="item.id"
                      class="p-2 flex bg-white hover:bg-gray-100 cursor-pointer border-b border-gray-100"
                      style=""
                    >
                      <div class="p-2 w-12">
                        <img :src="item.image" alt="img product" />
                      </div>
                      <div class="flex-auto text-sm w-32">
                        <div class="font-bold truncate">
                          {{ item.title }}
                        </div>
                        <span class="text-3.5 text-seconday font-semibold">
                          {{ item.price }}</span
                        >
                      </div>
                      <div class="flex flex-col w-18 font-medium items-end">
                        <div
                          class="w-4 h-4 mb-6 hover:bg-red-200 rounded-full cursor-pointer text-red-700"
                        >
                          <svg
                            xmlns="http://www.w3.org/2000/svg"
                            width="100%"
                            height="100%"
                            fill="none"
                            viewBox="0 0 24 24"
                            stroke="currentColor"
                            stroke-width="2"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            class="feather feather-trash-2"
                          >
                            <polyline points="3 6 5 6 21 6"></polyline>
                            <path
                              d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"
                            ></path>
                            <line x1="10" y1="11" x2="10" y2="17"></line>
                            <line x1="14" y1="11" x2="14" y2="17"></line>
                          </svg>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="flex justify-start items-start lg:hidden">
              <div class="relative">
                <div
                  class="flex flex-row cursor-pointer truncate p-2 px-4 rounded"
                  @mouseover="showMenu = !showMenu"
                  @blur="showMenu = false"
                >
                  <div class="flex flex-row-reverse ml-2 w-full">
                    <div class="relative">
                      <svg
                        class="fill-current w-8 h-8"
                        viewBox="0 0 512 512"
                        xmlns="http://www.w3.org/2000/svg"
                      >
                        <path
                          fill="#000"
                          d="M32 96v64h448V96H32zm0 128v64h448v-64H32zm0 128v64h448v-64H32z"
                        />
                      </svg>
                    </div>
                  </div>
                </div>
                <div
                  v-if="showMenu"
                  class="absolute w-full rounded-b border-t-0 z-99"
                >
                  <div class="shadow-xl w-32">
                    <div
                      v-for="item in quickMenu"
                      :key="item.id"
                      class="p-2 flex bg-white hover:bg-gray-100 cursor-pointer border-b border-gray-100"
                      style=""
                    >
                      <nuxt-link :to="item.slug" class="flex-auto text-sm w-32">
                        <div class="font-bold">{{ item.title }}</div>
                      </nuxt-link>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </ul>
        </nav>
        <div
          class="flex space-x-4 border-l ml-6 pl-6 border-gray-900/10 dark:border-gray-50/[0.2]"
        >
          <LanguageSwitcher />
          <ThemeSwitcher />
          <Anchor
            class="hover:no-underline hover:text-slate-900 hover:dark:text-white text-lg flex self-center items-center"
            href="https://github.com/viandwi24/nuxt3-awesome-starter"
            title="Github"
          >
            <IconMdi:github-face />
          </Anchor>
        </div>
      </div>
    </template>
    <template #options="{ toggleOptions }">
      <ActionSheet @onClose="toggleOptions(false)">
        <ActionSheetBody>
          <ActionSheetHeader text="Menu" />
          <nav class="leading-6 font-semibold text-gray-600 dark:text-gray-300">
            <ul class="flex flex-col">
              <li
                v-for="(item, i) in menus"
                :key="i"
                class="flex w-full"
                :class="{
                  'pb-2 mb-2 border-b border-gray-900/10 dark:border-gray-50/[0.2]':
                    item.type === 'link',
                }"
              >
                <Anchor
                  v-if="item.type === 'link'"
                  :to="item.route ? item.route : undefined"
                  :href="item.href ? item.href : undefined"
                  class="flex-1 hover:no-underline capitalize"
                  >{{ item.text }}</Anchor
                >
                <Button
                  v-else-if="item.type === 'button'"
                  :text="item.text"
                  size="xs"
                  class="flex-1 font-extrabold capitalize"
                  :to="item.route ? item.route : undefined"
                  :href="item.href ? item.href : undefined"
                />
              </li>
            </ul>
          </nav>
          <div class="mt-6 text-sm font-bold capitalize">
            {{ $t('components.theme_switcher.change_theme') }}
          </div>
          <div class="mt-2">
            <ThemeSwitcher type="select-box" />
          </div>
          <div class="mt-6 text-sm font-bold capitalize">
            {{ $t('components.language_switcher.change_language') }}
          </div>
          <div class="mt-2">
            <LanguageSwitcher type="select-box" />
          </div>
        </ActionSheetBody>
        <Button
          type="secondary"
          title="Github"
          href="https://github.com/viandwi24/nuxt3-awesome-starter"
        >
          <IconMdi:github-face />
          <span class="ml-1">Github</span>
        </Button>
        <Button
          text="Close"
          type="secondary"
          @click.prevent="toggleOptions(false)"
        />
      </ActionSheet>
    </template>
  </BuilderNavbar>
</template>
