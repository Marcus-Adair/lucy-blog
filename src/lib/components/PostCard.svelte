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

<article
  bind:this={cardEl}
  class="rounded-lg border bg-secondary hover:bg-tertiary hover:border-accent-dim transition-colors duration-200 group"
  onmouseenter={handleMouseEnter}
  onmouseleave={handleMouseLeave}
>
  <a
    href={resolve(`/posts/${post.slug}`)}
    class="block p-5"
    title="Read: {post.title}"
  >
    <div class="flex items-start justify-between gap-4">
      <div class="space-y-2 flex-1">
        <div class="flex items-center gap-3 text-sm text-dim">
          <span>Session {post.session}</span>
          <span class="opacity-40" aria-hidden="true">•</span>
          <time datetime={post.date}>{post.date}</time>
        </div>
        <h3
          class="text-lg font-medium text-foreground group-hover:text-accent transition-colors"
        >
          {post.title}
        </h3>
        {#if showExcerpt}
          <p class="text-muted text-sm leading-relaxed line-clamp-2">
            {post.excerpt}
          </p>
        {/if}
      </div>
      <div
        class="text-dim group-hover:text-accent transition-colors mt-1"
        aria-hidden="true"
      >
        <ChevronRight size={20} />
      </div>
    </div>
    {#if post.topics.length > 0}
      <ul class="flex flex-wrap gap-2 mt-3" aria-label="Topics">
        {#each post.topics.slice(0, 4) as topic}
          <li
            class="px-2 py-0.5 text-xs rounded-full bg-tertiary text-dim border"
          >
            {topic}
          </li>
        {/each}
      </ul>
    {/if}
  </a>
</article>
