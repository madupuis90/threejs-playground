<script lang="ts">
  export let title: string;
  export let object: any;
  export let color;
</script>

<div class="info">
  <fieldset>
    <legend>{title}</legend>
    <pre>
      <p style="color: {color};">{JSON.stringify(object, null, 2)}</p>
    </pre>
  </fieldset>
</div>

<style>
  .info {
    cursor: default;
  }
</style>
