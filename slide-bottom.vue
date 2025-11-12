  <template>
  <footer v-if="$nav.currentPage !== 1"
    class="absolute bottom-0 left-0 right-0 px-4 py-2 flex-items-baseline flex flex-justify-between z-36 font-bold text-sm"
  >
    <div class="flex gap-4">
      <p>
        <fa6-brands-square-bluesky class="text-sky text-lg m-r-1" />
        <a href="https://bsky.app/profile/dbodky.me" target="_blank">@dbodky.me</a>
      </p>
      <p>
        <fa6-brands-linkedin class="text-blue-600 text-lg m-r-1" />
        <a href="https://linkedin.com/in/daniel-bodky" target="_blank">in/daniel-bodky</a>
      </p>
    </div>
    <img class="max-h-8" src="/logo.png" alt="NETWAYS Managed Services logo" />
  </footer>
</template>
