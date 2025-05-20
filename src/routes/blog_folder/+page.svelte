<script>
  import blogs from "../../blogs/blog"
  import text_file from "../../assets/folders/text_file.png";
  import "./blog_folder.css";
  import { goto } from "$app/navigation";
  import BlogDisplay from "../../components/blog_display.svelte";
  import { onMount } from 'svelte';
  let selected = null;     // for visual selection
  let openedBlog = null;   // for actual display of blog content
  let blogContent = "";    // content of the blog once loaded

  function handleSelect(index) {
    selected = index;
  }

  async function openBlog(blog) {
    try {
      // Fetch the markdown file
      const response = await fetch(blog.filepath);
      if (!response.ok) {
        throw new Error(`Failed to load blog: ${response.status}`);
      }
      
      // Get the text content of the markdown file
      blogContent = await response.text();
      
      // Set the opened blog with content
      openedBlog = {
        ...blog,
        content: blogContent
      };
    } catch (error) {
      console.error("Error loading blog:", error);
      alert(`Failed to load blog: ${error.message}`);
    }
  }
</script>

<div class="window" style="width: 300px">
  <div class="title-bar">
    <div class="title-bar-text">Blog Folder</div>
    <div class="title-bar-controls">
      <button aria-label="Minimize"></button>
      <button aria-label="Maximize"></button>
      <button aria-label="Close" on:click={() => goto('/')}></button>
    </div>
  </div>

  <div class="window-body">
    {#each blogs as blog, index}
      <button
        class={selected === index ? "blog-entry-selected" : "blog-entry"}
        on:click={() => handleSelect(index)}
        on:dblclick={() => openBlog(blog)}
        on:keyup={(event) => {
          if (event.key === "Enter") openBlog(blog);
        }}
      >
        <img src={text_file} alt="text file icon" class="blog-icon" />
        <p class="blog-title">{blog.blogname}</p>
      </button>
    {/each}
  </div>
</div>

{#if openedBlog}
  <BlogDisplay 
    title={openedBlog.blogname} 
    date={openedBlog.date} 
    content={openedBlog.content} 
    close={() => openedBlog = null}
  />
{/if}