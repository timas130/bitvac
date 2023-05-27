<script>
  import "../app.css";

  export const prerender = true;
</script>

<div class="background min-h-screen text-white">
  <div class="max-w-3xl mx-auto py-4 px-3">
    <slot />
  </div>
</div>
