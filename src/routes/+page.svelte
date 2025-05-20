<script>
  import my_docs from "../assets/folders/my_docs.png"
  import image from "../assets/folders/image.png"
  import { goto } from "$app/navigation";

  
</script>

<div class="desktop-screen">
  <button class="blog" on:dblclick={() => {goto('/blog_folder')}}>
    <img src="{my_docs}" alt="My Documents" class="my-docs" />
    blog
  </button>

  <button class="blog" on:dblclick={() => {goto('/resume')}}>
    <img src="{image}" alt="resume" class="my-resume" />
    resume
  </button>

  <button class="blog" on:dblclick={() => {goto('/about')}}>
    <img src="{image}" alt="resume" class="my-resume" />
    kenz.exe
  </button>
</div>

<style>

  .desktop-screen {
    width: 20%;
    display: grid;
    grid-template-columns: repeat(2, 1fr); /* 12 columns */
    grid-template-rows: repeat(10, 60px);   /* 10 rows with fixed height */
    gap: 4px;
  }

  button {
    all:unset;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    color: white;
    font-weight: lighter;
  }


</style>
