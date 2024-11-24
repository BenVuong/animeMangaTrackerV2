<script lang="ts">
  import { onMount, onDestroy } from "svelte";
  import { currentUser, pb } from "./pocketbase";
  import * as Card from "$lib/components/ui/card";
  import Button from "./components/ui/button/button.svelte";
  import AnimeInfo from "./AnimeInfo.svelte";
  import AnimeEdit from "./AnimeEdit.svelte";
  let newMessage: string;
  let anime: any[] = [];
  let unsubscribe: () => void;

  onMount(async () => {
    // Get initial messages
    const resultList = await pb.collection("anime").getList(1, 50, {
      sort: "title",
      expand: "users",
    });
    anime = resultList.items;

    // Subscribe to realtime messages
    unsubscribe = await pb
      .collection("anime")
      .subscribe("*", async ({ action, record }) => {
        if (action === "create") {
          // Fetch associated user
          const user = await pb.collection("users").getOne(record.user);
          record.expand = { user };
          anime = [...anime, record];
        }
        if (action === "delete") {
          anime = anime.filter((a) => a.id !== record.id);
        }
        if (action === "update") {
          const resultList = await pb.collection("anime").getList(1, 50, {
            sort: "title",
            expand: "users",
          });
          anime = resultList.items;
        }
      });
  });

  // Unsubscribe from realtime messages
  onDestroy(() => {
    unsubscribe?.();
  });

  async function sendMessage() {
    const data = {
      text: newMessage,
      user: $currentUser!.id,
    };
    const createdMessage = await pb.collection("messages").create(data);
    newMessage = "";
  }
</script>

<div class="grid grid-cols-1 md:grid-cols-4">
  {#each anime as anime (anime.id)}
    <Card.Root>
      <Card.Header>
        <Card.Title>{anime.title}</Card.Title>
        <Card.Description></Card.Description>
      </Card.Header>
      <Card.Content>
        <p>Episodes Watched: {anime.episodeWatchedCount}</p>
        <p>Total Episode Count: {anime.totalEpisodeCount}</p>
        Type: {anime.type}
      </Card.Content>
      <Card.Footer>
        <p>
          <AnimeInfo {anime}></AnimeInfo><AnimeEdit {anime} /><Button
            >Delete</Button
          >
        </p>
      </Card.Footer>
    </Card.Root>
  {/each}
</div>
