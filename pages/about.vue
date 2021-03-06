<template>
  <div>
    <section class="section">
      <div class="container">
        <div class="content">
          <h2 class="title">About</h2>
          <div class="about" v-for="about in abouts" :key="about.id">
            <div class="thmb">
              <img :src="about.icon.url" />
            </div>
            <p class="name">{{ about.name }}</p>
            <p class="description">{{ about.description }}</p>
            <div class="tags">
              <a
                v-if="about.githubLink"
                class="tag is-light"
                :href="about.githubLink"
                target="_blank"
                rel="nofollow noopener"
              >
                <span class="icon">
                  <fa :icon="faGithub" />
                </span>
                <span>GitHub</span>
              </a>
              <a
                v-if="about.twitterLink"
                class="tag is-info is-light"
                :href="about.twitterLink"
                target="_blank"
                rel="nofollow noopener"
              >
                <span class="icon">
                  <fa :icon="faTwitter" />
                </span>
                <span>Twitter</span>
              </a>
              <a
                v-if="about.qiitaLink"
                class="tag is-success is-light"
                :href="about.qiitaLink"
                target="_blank"
                rel="nofollow noopener"
              >
                <span class="icon">
                  <fa :icon="faSearch" />
                </span>
                <span>Qiita</span>
              </a>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
.name {
  text-align: center;
}
.description {
  white-space: pre-wrap;
}
.thmb {
  position: relative;
  width: 80px;
  height: 80px;
  border-radius: 50%;
  border: 1px solid rgba(0,0,0, 0.05);
  margin: 0 auto;
}
.thmb > img {
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: cover;
  clip-path: circle(50%);
}
.tags {
  display: flex;
  align-items: center;
  filter: drop-shadow(0 3px 8px var(--main-shadow));
}
@media screen and (min-width: 1024px) {
  .thmb {
    margin: 0
  }
  .name {
    text-align: initial;
  }
}
</style>

<script lang="ts">
import Vue from 'vue'
import { graphQLClient, gql } from "../api"
import { faTwitter, faGithub } from "@fortawesome/free-brands-svg-icons"
import { faSearch } from "@fortawesome/free-solid-svg-icons"

type Asset = {
  id: string
  url: string
}

type About = {
  name: string
  description: string
  icon: Asset
  githubLink?: string
  twitterLink?: string
  qiitaLink?: string
}

export default Vue.extend({
  asyncData: async () => {
    const query = gql`
      {
        abouts {
          id
          name
          description
          icon {
            id
            url
          }
          githubLink
          twitterLink
          qiitaLink
        }
      }
    `
    const { abouts } = await graphQLClient.request<{ abouts: About[]}>(query)

    return {
      abouts
    }
  },
  data: () => ({
    faTwitter,
    faGithub,
    faSearch,
  })
})
</script>
