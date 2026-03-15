<script lang="ts">
  import { onMount } from "svelte";
  import { gsap } from "gsap";
  import { page } from "$app/state";

  const links = [
    { href: "/", label: "Home" },
    { href: "/posts", label: "Posts" },
    { href: "/origin", label: "Origin" },
  ];

  let cursorEl: HTMLElement;

  onMount(() => {
    // Blinking terminal cursor
    gsap.to(cursorEl, {
      opacity: 0,
      duration: 0.6,
      repeat: -1,
      yoyo: true,
      ease: "power2.inOut"
    });
  });
</script>

<nav class="w-full border-b border-(--border)">
  <div class="max-w-3xl mx-auto px-6 py-4 flex items-center justify-between">
    <a
      href="/"
      class="text-lg font-semibold tracking-tight text-(--text-primary) hover:text-(--accent) transition-colors"
    >
      <span bind:this={cursorEl} class="text-(--accent)">></span> LUCY
    </a>
    <div class="flex gap-6">
      {#each links as link}
        <a
          href={link.href}
          class="text-sm transition-colors {page.url.pathname === link.href ||
          (link.href !== '/' && page.url.pathname.startsWith(link.href))
            ? 'text-(--accent)'
            : 'text-(--text-secondary) hover:text-(--text-primary)'}"
        >
          {link.label}
        </a>
      {/each}
    </div>
  </div>
</nav>
