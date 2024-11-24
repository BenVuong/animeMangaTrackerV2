<script lang="ts">
  export let anime;

  import * as Dialog from "$lib/components/ui/dialog";
  import Input from "./components/ui/input/input.svelte";
  import { currentUser, pb } from "./pocketbase";
  let newEpisodeWatchedCount: number = anime.episodeWatchedCount;
  let newTotalEpisodeCount: number = anime.totalEpisodeCount;
  let id: string = anime.id;
  let newTitle: string = anime.title;
  let newSynopsis: string = anime.synopsis;
  let newStudo: string = anime.studio;
  let newType: string = anime.type;
  async function editAnimeInfo() {
    const data = {
      title: newTitle,
      totalEpisodeCount: newTotalEpisodeCount,
      episodeWatchedCount: newEpisodeWatchedCount,
      synopsis: newSynopsis,
      studio: newStudo,
      user: $currentUser!.id,
      type: newType,
    };
    const updateAnime = await pb.collection("anime").update(id, data);
  }
</script>

<Dialog.Root>
  <Dialog.Trigger>Update</Dialog.Trigger>
  <Dialog.Content>
    <Dialog.Header>
      <Dialog.Title>{anime.title}</Dialog.Title>
      <Dialog.Description>Update entry</Dialog.Description>
    </Dialog.Header>
    <form on:submit|preventDefault={editAnimeInfo}>
      Episodes Watched
      <Input type="number" bind:value={newEpisodeWatchedCount} />
      Total Episodes
      <Input type="number" bind:value={newTotalEpisodeCount} />
      <Dialog.Trigger><button type="submit">Update</button></Dialog.Trigger>
    </form>
  </Dialog.Content>
</Dialog.Root>
