<script lang="ts">
  import * as AlertDialog from "$lib/components/ui/alert-dialog";
  import Button from "./components/ui/button/button.svelte";
  import Input from "./components/ui/input/input.svelte";

  import { currentUser, pb } from "./pocketbase";
  let newEpisodeWatchedCount: number;
  let newTotalEpisodeCount: number;

  let newTitle: string;
  let newSynopsis: string;
  let newStudo: string;
  let newType: string;

  async function createAnimeInfo() {
    const data = {
      title: newTitle,
      totalEpisodeCount: newTotalEpisodeCount,
      episodeWatchedCount: newEpisodeWatchedCount,
      synopsis: newSynopsis,
      studio: newStudo,
      user: $currentUser!.id,
      type: newType,
    };
    const createAnime = await pb.collection("anime").create(data);
    newTitle = "";
    newTotalEpisodeCount = 0;
    newEpisodeWatchedCount = 0;
    newSynopsis = "";
    newStudo = "";
    newType = "";
  }
</script>

<Button>
  <AlertDialog.Root>
    <AlertDialog.Trigger>Create</AlertDialog.Trigger>
    <AlertDialog.Content>
      <AlertDialog.Header>
        <AlertDialog.Title>Create new entry</AlertDialog.Title>
        <AlertDialog.Description></AlertDialog.Description>
      </AlertDialog.Header>
      <form on:submit|preventDefault={createAnimeInfo}>
        Title
        <Input type="text" bind:value={newTitle} />
        Episodes Watched
        <Input type="number" bind:value={newEpisodeWatchedCount} />
        Total Episodes
        <Input type="number" bind:value={newTotalEpisodeCount} />

        <AlertDialog.Footer>
          <AlertDialog.Cancel>Cancel</AlertDialog.Cancel>
          <AlertDialog.Action
            ><button type="submit">Create</button></AlertDialog.Action
          >
        </AlertDialog.Footer>
      </form>
    </AlertDialog.Content>
  </AlertDialog.Root>
</Button>
