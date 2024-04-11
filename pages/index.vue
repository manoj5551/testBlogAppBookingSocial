<template>
    <main>
        <BlogHero />
        <Section id="main" class="!pt-0">
            <ContentQuery
                path="/blog/"
                :only="['headline', 'excerpt', 'date', 'tags', '_path', 'image']"
                :sort="{
                    date: -1
                }"
                :limit="blogCountLimit"
                v-slot="{ data }"
            >
                <BlogList :data="data" />
            </ContentQuery>
            <BlogPagination
                v-if="data > 1"
                class="mt-8"
                :currentPage="1"
                :totalPages="data"
                :nextPage="data > 1"
                baseUrl="/"
                pageUrl="/page/"
            />
        </Section>
    </main>
</template>

<script setup>
// Find the number of blogs present
const blogCountLimit = 6;
const { data } = await useAsyncData(`content-/`, async () => {
    const _posts = await queryContent('/').only('headline').find()
    return Math.ceil(_posts.length / blogCountLimit);
});
</script>
