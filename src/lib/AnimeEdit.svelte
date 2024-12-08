<script lang="ts">
  export let anime;
  import * as AlertDialog from "$lib/components/ui/alert-dialog";
  import Input from "./components/ui/input/input.svelte";
  import { currentUser, pb } from "./pocketbase";
  import { Textarea } from "$lib/components/ui/textarea";
  let updatedEpisodeWatchedCount: number = anime.episodeWatchedCount;
  let updatedTotalEpisodeCount: number = anime.totalEpisodeCount;
  let id: string = anime.id;
  let updatedTitle: string = anime.title;
  let updatedSynopsis: string = anime.synopsis;
  let updatedStudo: string = anime.studio;
  let updatedType: string = anime.type;
  let updatedCoverArt: string = anime.coverArt;
  const selectableTypes = [
    { value: "TV", label: "TV" },
    { value: "Movie", label: "Movie" },
    { value: "OVA", label: "OVA" },
    { value: "ONA", label: "ONA" },
  ];
  async function editAnimeInfo() {
    const data = {
      title: updatedTitle,
      totalEpisodeCount: updatedTotalEpisodeCount,
      episodeWatchedCount: updatedEpisodeWatchedCount,
      synopsis: updatedSynopsis,
      studio: updatedStudo,
      user: $currentUser!.id,
      type: updatedType,
      coverArt: updatedCoverArt,
    };
    const updateAnime = await pb.collection("anime").update(id, data);
  }
</script>

<AlertDialog.Root>
  <AlertDialog.Trigger>Update</AlertDialog.Trigger>
  <AlertDialog.Content>
    <AlertDialog.Header>
      <AlertDialog.Title>{anime.title}</AlertDialog.Title>
      <AlertDialog.Description>Update entry information</AlertDialog.Description
      >
    </AlertDialog.Header>
    <form on:submit|preventDefault={editAnimeInfo}>
      Title
      <Input type="text" bind:value={updatedTitle} />
      Episodes Watched
      <Input type="number" bind:value={updatedEpisodeWatchedCount} />
      Total Episodes
      <Input type="number" bind:value={updatedTotalEpisodeCount} />
      Synopsis
      <Textarea bind:value={updatedSynopsis} />
      Studio
      <Input type="text" bind:value={updatedStudo} />
      Cover Art
      <Input type="text" bind:value={updatedCoverArt} />
      Type
      <select bind:value={updatedType}>
        {#each selectableTypes as { value, label }}
          <option {value}>{label}</option>
        {/each}
      </select>
      <AlertDialog.Footer>
        <AlertDialog.Cancel>Cancel</AlertDialog.Cancel>
        <AlertDialog.Action
          ><button type="submit">Update</button></AlertDialog.Action
        >
      </AlertDialog.Footer>
    </form>
  </AlertDialog.Content>
</AlertDialog.Root>
