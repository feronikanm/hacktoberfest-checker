<template>
  <li class="Repository">
    <div>
      <div class="Repository__Row">
        <h3 class="flex Repository__Name">
          <svg v-if="theyHacktoberfest" class="w-6 mr-2 text-teal-500 fill-current" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
            <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
          </svg>
          <svg v-else class="w-6 mr-2 text-red-500 fill-current" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
            <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zM8.707 7.293a1 1 0 00-1.414 1.414L8.586 10l-1.293 1.293a1 1 0 101.414 1.414L10 11.414l1.293 1.293a1 1 0 001.414-1.414L11.414 10l1.293-1.293a1 1 0 00-1.414-1.414L10 8.586 8.707 7.293z" clip-rule="evenodd" />
          </svg>
          <a :href="repo.url">{{ repo.name }}</a>
        </h3>
      </div>
      <div class="flex Repository__Row">
        <p class="flex-initial pr-10">
          {{ repo.description }}<br />
          <ul v-if="theyHacktoberfest" class="mt-2 ml-8 list-disc">
            <li v-if="repo.topic"
              ><code>hacktoberfest</code> topic</li
            >
            <li v-if="repo.tag_prs"
              ><code>hacktoberfest-accepted</code> PRs</li
            >
          </ul>
        </p>
        <div class="flex-1 pl-10 text-right">
          <div class="inline-flex">
            <button
              class="Button Button__Remove"
              @click="$emit('remove', $vnode.key)"
            >
              Remove
            </button>
            <button
              class="Button Button__Refresh"
              @click="$emit('refresh', $vnode.key)"
            >
              Refresh
            </button>
          </div>
        </div>
      </div>

      <div v-if="repo.topics" class="Repository__Row">
        <a
          v-for="(topic, index) in repo.topics"
          :key="index"
          :href="`https://github.com/topics/${topic}`"
        >
          {{ topic }}
        </a>
      </div>

      <div class="flex space-x-10 text-sm Repository__Row">
        <span v-if="repo.language">
          <span
            v-if="colors[repo.language]"
            class="Language__Color"
            :style="`background-color: ${colors[repo.language].color}`"
          ></span>
          <span>{{ repo.language }} </span>
        </span>

        <a v-if="repo.stargazers_count > 0" :href="`${repo.url}/stargazers`">
          <svg
            aria-label="star"
            viewBox="0 0 16 16"
            version="1.1"
            width="16"
            height="16"
            role="img"
            class="inline mr-2"
          >
            <path
              fill-rule="evenodd"
              d="M8 .25a.75.75 0 01.673.418l1.882 3.815 4.21.612a.75.75 0 01.416 1.279l-3.046 2.97.719 4.192a.75.75 0 01-1.088.791L8 12.347l-3.766 1.98a.75.75 0 01-1.088-.79l.72-4.194L.818 6.374a.75.75 0 01.416-1.28l4.21-.611L7.327.668A.75.75 0 018 .25zm0 2.445L6.615 5.5a.75.75 0 01-.564.41l-3.097.45 2.24 2.184a.75.75 0 01.216.664l-.528 3.084 2.769-1.456a.75.75 0 01.698 0l2.77 1.456-.53-3.084a.75.75 0 01.216-.664l2.24-2.183-3.096-.45a.75.75 0 01-.564-.41L8 2.694v.001z"
            ></path>
          </svg>
          {{ repo.stargazers_count }}
        </a>

        <a v-if="repo.forks > 0" :href="`${repo.url}/network/members`">
          <svg
            aria-label="fork"
            viewBox="0 0 16 16"
            version="1.1"
            width="16"
            height="16"
            role="img"
            class="inline mr-2"
          >
            <path
              fill-rule="evenodd"
              d="M5 3.25a.75.75 0 11-1.5 0 .75.75 0 011.5 0zm0 2.122a2.25 2.25 0 10-1.5 0v.878A2.25 2.25 0 005.75 8.5h1.5v2.128a2.251 2.251 0 101.5 0V8.5h1.5a2.25 2.25 0 002.25-2.25v-.878a2.25 2.25 0 10-1.5 0v.878a.75.75 0 01-.75.75h-4.5A.75.75 0 015 6.25v-.878zm3.75 7.378a.75.75 0 11-1.5 0 .75.75 0 011.5 0zm3-8.75a.75.75 0 100-1.5.75.75 0 000 1.5z"
            ></path>
          </svg>
          {{ repo.forks }}
        </a>

        <span v-if="repo.license">
          <svg
            viewBox="0 0 16 16"
            version="1.1"
            width="16"
            height="16"
            role="img"
            class="inline mr-2"
          >
            <path
              fill-rule="evenodd"
              d="M8.75.75a.75.75 0 00-1.5 0V2h-.984c-.305 0-.604.08-.869.23l-1.288.737A.25.25 0 013.984 3H1.75a.75.75 0 000 1.5h.428L.066 9.192a.75.75 0 00.154.838l.53-.53-.53.53v.001l.002.002.002.002.006.006.016.015.045.04a3.514 3.514 0 00.686.45A4.492 4.492 0 003 11c.88 0 1.556-.22 2.023-.454a3.515 3.515 0 00.686-.45l.045-.04.016-.015.006-.006.002-.002.001-.002L5.25 9.5l.53.53a.75.75 0 00.154-.838L3.822 4.5h.162c.305 0 .604-.08.869-.23l1.289-.737a.25.25 0 01.124-.033h.984V13h-2.5a.75.75 0 000 1.5h6.5a.75.75 0 000-1.5h-2.5V3.5h.984a.25.25 0 01.124.033l1.29.736c.264.152.563.231.868.231h.162l-2.112 4.692a.75.75 0 00.154.838l.53-.53-.53.53v.001l.002.002.002.002.006.006.016.015.045.04a3.517 3.517 0 00.686.45A4.492 4.492 0 0013 11c.88 0 1.556-.22 2.023-.454a3.512 3.512 0 00.686-.45l.045-.04.01-.01.006-.005.006-.006.002-.002.001-.002-.529-.531.53.53a.75.75 0 00.154-.838L13.823 4.5h.427a.75.75 0 000-1.5h-2.234a.25.25 0 01-.124-.033l-1.29-.736A1.75 1.75 0 009.735 2H8.75V.75zM1.695 9.227c.285.135.718.273 1.305.273s1.02-.138 1.305-.273L3 6.327l-1.305 2.9zm10 0c.285.135.718.273 1.305.273s1.02-.138 1.305-.273L13 6.327l-1.305 2.9z"
            ></path></svg
          >{{ repo.license.name }}
        </span>

        <span>
          Requested
          <span
            :datetime="repo.requested_at"
            :title="new Date(repo.requested_at).toString()"
            >{{ repo.requested_at | moment('from') }}</span
          >
        </span>
      </div>
    </div>
  </li>
</template>

<script>
import colors from '@/data/colors.json'

export default {
  props: {
    repo: {
      type: Object,
      required: true,
    },
  },

  data() {
    return {
      colors,
    }
  },

  computed: {
    theyHacktoberfest() {
      return this.repo.topic || this.repo.tag_prs
    },
  },
}
</script>

<style scoped>
.Repository {
  @apply border-t text-left py-4;
}

.Repository:last-child {
  @apply border-b;
}

.Repository__Row {
  @apply my-2;
}

.Repository__Name {
  @apply text-xl;
}

.Button {
  @apply py-1 px-2 font-bold border-2 text-sm;
  border-color: var(--color-tertiary);
}

.Button:focus {
  @apply outline-none;
}

.Button:hover {
  background-color: var(--bg-secondary);
}

.Button__Remove {
  @apply border-r-0 rounded-l;
}

.Button__Refresh {
  @apply border-l-0 rounded-r;
}

.Language__Color {
  position: relative;
  top: 1px;
  display: inline-block;
  width: 12px;
  height: 12px;
  border-radius: 50%;
}

.Repository__Row code {
  @apply border px-1
}
</style>
