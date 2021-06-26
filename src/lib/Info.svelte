<script lang="ts">
  export let title: string;
  export let object: any;
</script>

<div class="info">
  <fieldset>
    <legend>{title}</legend>
    <pre>
      <p>{JSON.stringify(object, null, 2)}</p>
    </pre>
  </fieldset>
</div>

<style>
  .info {
    color: lime;
    cursor: default;
  }
</style>
