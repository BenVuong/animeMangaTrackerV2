<script lang="ts">
  export let anime;
  import * as Dialog from "$lib/components/ui/dialog";
</script>

<Dialog.Root>
  <Dialog.Trigger>More Info</Dialog.Trigger>
  <Dialog.Content>
    <Dialog.Header>
      <Dialog.Title>{anime.title}</Dialog.Title>
      <Dialog.Description></Dialog.Description>
    </Dialog.Header>
    <pr>{anime.synopsis}</pr>
  </Dialog.Content>
</Dialog.Root>
