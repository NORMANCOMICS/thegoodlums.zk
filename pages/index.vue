<script setup lang="ts">
import type { NavItem } from '@nuxt/content/types';

const { data: page } = await useAsyncData('index', () => queryContent('/').findOne());

const navigation = inject<Ref<NavItem[]>>('navigation');

const guides = computed(() => {
  const tutorialPath = navigation?.value.find((item) => item._path === '/tutorials') ?? { children: [] };

  return tutorialPath.children?.filter((tutorial) => tutorial.featured);
});

useSeoMeta({
  titleTemplate: '',
  title: page.value?.title,
});
</script>

<template>
  <div>
    <IconOrbit class="absolute hidden md:block" />
    <ULandingSection
      class="relative"
      title=""
      description="Build Together: Discover Community-Driven Guides and Tutorials for zkSync"
      :links="[
        {
          label: 'Check out all the tutorials',
          icon: 'i-zksync-zksync-logo',
          trailingIcon: 'i-heroicons-arrow-right-20-solid',
          to: '/tutorials',
          size: 'xl',
        },
      ]"
      :ui="{ description: 'backdrop-blur bg-background/65' }"
    >
      <template #headline>
        <NuxtImg
          src="/logos/zksync-icon.svg"
          width="240"
          class="invert filter dark:filter-none"
        />
      </template>

      <UPageGrid>
        <ULandingCard
          v-for="(guide, index) of guides"
          :key="index"
          :to="guide._path"
          :title="guide.title"
          :description="guide.summary"
          :ui="{ body: { base: 'justify-between' } }"
        >
          <div class="mt-auto">
            <AuthorsList :authors="guide.authors" />
            <div class="mt-2">
              <UBadge
                v-for="(tag, index) of guide.tags"
                :key="index"
                :label="tag"
                color="blue"
                size="xs"
                variant="subtle"
                class="mr-2"
              />
            </div>
          </div>
        </ULandingCard>
      </UPageGrid>
    </ULandingSection>
  </div>
</template>
