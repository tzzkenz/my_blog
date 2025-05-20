<script>
  import { goto } from "$app/navigation";
  import resume from "../../assets/resume/resume.jpg"
</script>

<div class="window" style="width: 500px">
  <div class="title-bar">
    <div class="title-bar-text">Kenz Resume</div>
    <div class="title-bar-controls">
      <button aria-label="Close" onclick={() => {goto('/')}}></button>
    </div>
  </div>
  <div class="window-body">
    <img src={resume} alt="Resume" class="resume" width=475/>
  </div>
</div>