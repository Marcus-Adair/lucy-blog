<script lang="ts">
  import { gsap } from "gsap";
  import { ChevronRight } from "lucide-svelte";
  import type { Post } from "$lib/data/posts";
  import { resolve } from "$app/paths";

  interface Props {
    post: Post;
    showExcerpt?: boolean;
  }

  let { post, showExcerpt = true }: Props = $props();

  let cardEl: HTMLElement;

  function handleMouseEnter() {
    gsap.to(cardEl, {
      scale: 1.02,
      duration: 0.25,
      ease: "power2.out",
    });
  }

  function handleMouseLeave() {
    gsap.to(cardEl, {
      scale: 1,
      duration: 0.25,
      ease: "power2.out",
    });
  }
</script>

<a
  bind:this={cardEl}
  href={resolve(`/posts/${post.slug}`)}
  class="block p-5 rounded-lg border border-(--border) bg-(--bg-secondary) hover:bg-(--bg-tertiary) hover:border-(--accent-dim) transition-colors duration-200 group"
  onmouseenter={handleMouseEnter}
  onmouseleave={handleMouseLeave}
>
  <div class="flex items-start justify-between gap-4">
    <div class="space-y-2 flex-1">
      <div class="flex items-center gap-3 text-sm text-(--text-muted)">
        <span>Session {post.session}</span>
        <span class="opacity-40">•</span>
        <span>{post.date}</span>
      </div>
      <h3
        class="text-lg font-medium text-(--text-primary) group-hover:text-(--accent) transition-colors"
      >
        {post.title}
      </h3>
      {#if showExcerpt}
        <p class="text-(--text-secondary) text-sm leading-relaxed line-clamp-2">
          {post.excerpt}
        </p>
      {/if}
    </div>
    <div
      class="text-(--text-muted) group-hover:text-(--accent) transition-colors mt-1"
    >
      <ChevronRight size={20} />
    </div>
  </div>
  {#if post.topics.length > 0}
    <div class="flex flex-wrap gap-2 mt-3">
      {#each post.topics.slice(0, 4) as topic}
        <span
          class="px-2 py-0.5 text-xs rounded-full bg-(--bg-tertiary) text-(--text-muted) border border-(--border)"
        >
          {topic}
        </span>
      {/each}
    </div>
  {/if}
</a>
