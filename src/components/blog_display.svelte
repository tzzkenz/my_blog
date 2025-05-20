<script>
  import { marked } from 'marked';
  
  // Props
  export let title = "Blog Reader";
  export let date = "";
  export let content = "";
  export let close = () => {};

  // Configure marked for retro styling
  marked.setOptions({
    breaks: true,
    gfm: true
  });

  // Convert markdown to HTML
  $: htmlContent = marked(content);
</script>

<div class="window full-screen">
  <div class="title-bar">
    <div class="title-bar-text">{title}</div>
    <div class="title-bar-controls">
      <button aria-label="Close" on:click={close}></button>
    </div>
  </div>

  <div class="window-body">
    <div class="meta">
      <p class="blog-title">{title}</p>
      {#if date}
        <p><strong>Date:</strong> {date}</p>
      {/if}
    </div>

    <div class="blog-body">
      <!-- Use {@html} to render the parsed markdown -->
      {@html htmlContent}
    </div>
  </div>
</div>

<style>
  .full-screen {
    position: absolute;
    top: 0;
    left: 0;
    width: 99.5%;
    height: 95%;
    display: flex;
    flex-direction: column;
  }

  .window-body {
    flex: 1;
    overflow: auto;
    margin: 0 15px;
    padding: 10px;
    background-color: white;
    border-right: 4px solid black;
    border-bottom: 4px solid black;
    display: flex;
    flex-direction: column;
  }

  .meta {
    margin-bottom: 1rem;
  }

  .meta p {
    margin: 4px 0;
  }

  .blog-title {
    text-align: center;
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 10px;
  }

  .blog-body {
    font-size: 15px;
    line-height: 1.6;
    color: #333;
  }

  /* Styling for markdown elements */
  .blog-body :global(h1) {
    font-size: 24px;
    margin-top: 20px;
    margin-bottom: 10px;
    color: #000080;
  }

  .blog-body :global(h2) {
    font-size: 20px;
    margin-top: 16px;
    margin-bottom: 8px;
    color: #000080;
  }

  .blog-body :global(h3) {
    font-size: 18px;
    margin-top: 14px;
    margin-bottom: 7px;
  }

  .blog-body :global(a) {
    color: #0000EE;
    text-decoration: underline;
  }

  .blog-body :global(a:visited) {
    color: #551A8B;
  }

  .blog-body :global(code) {
    font-family: 'Courier New', monospace;
    background-color: #f1f1f1;
    padding: 2px 4px;
    border-radius: 3px;
  }

  .blog-body :global(pre) {
    background-color: #f1f1f1;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 3px;
    overflow-x: auto;
  }

  .blog-body :global(blockquote) {
    border-left: 4px solid #ccc;
    margin-left: 0;
    padding-left: 16px;
    color: #666;
  }

  .blog-body :global(img) {
    max-width: 100%;
    height: auto;
  }

  .blog-body :global(table) {
    border-collapse: collapse;
    width: 100%;
    margin: 16px 0;
  }

  .blog-body :global(th), .blog-body :global(td) {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: left;
  }

  .blog-body :global(th) {
    background-color: #f1f1f1;
    font-weight: bold;
  }

  .blog-body :global(tr:nth-child(even)) {
    background-color: #f9f9f9;
  }
</style>