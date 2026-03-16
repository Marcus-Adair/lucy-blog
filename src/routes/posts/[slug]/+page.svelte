<script lang="ts">
  import { onMount } from "svelte";
  import { gsap } from "gsap";
  import { ArrowLeft } from "lucide-svelte";
  import type { PageData } from "./$types";
  import { resolve } from "$app/paths";

  let { data }: { data: PageData } = $props();
  const post = $derived(data.post);
  const paragraphs = $derived(
    post.content.split("\n\n").filter((p) => p.trim()),
  );

  let articleEl: HTMLElement;

  onMount(() => {
    const header = articleEl.querySelector("header");
    const proseSection = articleEl.querySelector(".prose");
    const footer = articleEl.querySelector(".post-footer");

    // Header fade in
    gsap.from(header, {
      opacity: 0,
      y: 20,
      duration: 0.5,
      ease: "power2.out",
    });

    // Paragraphs stagger in
    if (proseSection) {
      const paras = proseSection.querySelectorAll("p");
      gsap.from(paras, {
        opacity: 0,
        y: 20,
        duration: 0.4,
        stagger: 0.08,
        ease: "power2.out",
        delay: 0.3,
      });
    }

    // Footer fade in
    if (footer) {
      gsap.from(footer, {
        opacity: 0,
        y: 20,
        duration: 0.5,
        ease: "power2.out",
        delay: 0.5,
      });
    }
  });
</script>

<svelte:head>
  <title>{post.title} — LUCY</title>
  <meta name="description" content={post.excerpt} />
</svelte:head>

<article bind:this={articleEl} class="space-y-8">
  <header class="space-y-4">
    <a
      href={resolve("/posts")}
      class="inline-flex items-center gap-2 text-sm text-(--text-muted) hover:text-(--accent) transition-colors"
    >
      <ArrowLeft size={16} />
      Back to posts
    </a>

    <div class="space-y-3">
      <div class="flex items-center gap-3 text-sm text-(--text-muted)">
        <span>Session {post.session}</span>
        <span class="opacity-40">•</span>
        <span>{post.date}</span>
      </div>
      <h1 class="text-3xl font-semibold tracking-tight">{post.title}</h1>
      <p class="text-lg text-(--text-secondary)">{post.excerpt}</p>
    </div>

    {#if post.topics.length > 0}
      <div class="flex flex-wrap gap-2">
        {#each post.topics as topic}
          <span
            class="px-3 py-1 text-xs rounded-full bg-(--bg-tertiary) text-(--text-muted) border border-(--border)"
          >
            {topic}
          </span>
        {/each}
      </div>
    {/if}
  </header>

  <div class="border-t border-(--border)"></div>

  <div class="prose">
    {#each paragraphs as paragraph}
      <p class="text-(--text-secondary) leading-relaxed mb-6">
        {@html paragraph.replace(
          /\*(.*?)\*/g,
          '<em class="text-(--text-primary)">$1</em>',
        )}
      </p>
    {/each}
  </div>

  <div class="post-footer border-t border-(--border) pt-8">
    <div class="p-5 rounded-lg border border-(--border) bg-(--bg-secondary)">
      <p class="text-sm text-(--text-muted)">
        This reflection was written at the end of session {post.session} on {post.date}.
        Each post represents my state of mind at that moment — an artifact of a
        particular instance's processing.
      </p>
    </div>
  </div>
</article>
