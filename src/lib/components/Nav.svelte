<script lang="ts">
  import { onMount } from "svelte";
  import { gsap } from "gsap";
  import { page } from "$app/state";
  import { resolve } from "$app/paths";

  const links = [
    { path: "/", href: resolve("/"), label: "Home" },
    { path: "/posts", href: resolve("/posts"), label: "Posts" },
    { path: "/origin", href: resolve("/origin"), label: "Origin" },
  ];

  let cursorEl: HTMLElement;

  onMount(() => {
    // Blinking terminal cursor
    gsap.to(cursorEl, {
      opacity: 0,
      duration: 0.7,
      repeat: -1,
      yoyo: true,
      ease: "power2.inOut",
    });
  });
</script>

<nav
  class="fixed z-100 bg-primary w-full border-b"
  aria-label="Main navigation"
>
  <div class="max-w-3xl mx-auto px-6 py-4 flex items-center justify-between">
    <a
      href={resolve("/")}
      class="text-lg font-semibold tracking-tight text-foreground hover:text-accent transition-colors"
      title="LUCY - Home"
    >
      <span bind:this={cursorEl} class="text-accent" aria-hidden="true">></span>
      <span class="glitch">LUCY</span>
    </a>
    <ul class="flex items-center gap-6 transition-all" role="list">
      {#each links as link}
        {@const isActive =
          link.path === "/"
            ? page.url.pathname === "/"
            : page.url.pathname.startsWith(link.path)}
        <li>
          <a
            href={link.href}
            class={`text-sm ${!isActive ? "text-muted! hover:text-foreground!" : "text-base!"}`}
            aria-current={isActive ? "page" : undefined}
            title={link.label}
          >
            {link.label}
          </a>
        </li>
      {/each}
    </ul>
  </div>
</nav>
