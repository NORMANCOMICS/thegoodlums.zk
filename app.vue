<script setup lang="ts">
import type { ParsedContent } from '@nuxt/content/types';

provideHeadlessUseId(() => useId());
const { seo } = useAppConfig();

const { data: navigation } = await useAsyncData('navigation', () => fetchContentNavigation());
provide('navigation', navigation);

useHead({
  meta: [{ name: 'viewport', content: 'width=device-width, initial-scale=1' }],
  link: [{ rel: 'icon', href: '/favicon.ico' }],
  htmlAttrs: {
    lang: 'en',
  },
});

useSeoMeta({
  titleTemplate: `%s - ${seo?.siteName}`,
  ogSiteName: seo?.siteName,
  ogUrl: 'https://code.zksync.io/',
  description:
    'Build together with the zkSync Community. Learn how to build amazing smart contracts and dApps on zkSync Era.',
  ogDescription:
    'Build together with the zkSync Community. Learn how to build amazing smart contracts and dApps on zkSync Era.',
  twitterTitle: `%s`,
  twitterDescription:
    'Build together with the zkSync Community. Learn how to build amazing smart contracts and dApps on zkSync Era.',
  twitterCard: 'summary_large_image',
  twitterSite: '@zksync',
  twitterCreator: '@zkSyncDevs',
  twitterImageAlt: 'Hyperscaling Ethereum with ZK tech.',
});

defineOgImageComponent('OgImageZK');

const { data: files } = useLazyFetch<ParsedContent[]>('/api/search.json', {
  default: () => [],
  server: false,
});
</script>

<template>
  <div>
    <NuxtLoadingIndicator />

    <HeaderComponent :search="true" />

    <UMain>
      <NuxtLayout>
        <NuxtPage />
      </NuxtLayout>
    </UMain>

    <FooterComponent />

    <ClientOnly>
      <LazyUContentSearch
        :files="files"
        :navigation="navigation || []"
      />
    </ClientOnly>

    <UNotifications />
  </div>
</template>
