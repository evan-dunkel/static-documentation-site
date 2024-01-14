<!--
  @component
  **Card**  
  This is a simple Card component.

  Usage:
  ```
    <Card href="https://url.com" title="Title" description="" imgURL="">
  ```

-->
<script>
    export let href = undefined;
    export let title = "";
    export let description = "";
    export let imgURL = undefined;
</script>
<li>
    <a {href}>
        <img href={imgURL} alt="">
        <h2>{title}</h2>
        <p>{description}</p>
    </a>
</li>