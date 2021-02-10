<script>
  import Header from "./UI/Header.svelte";
  import MeetupGrid from "./Meetups/MeetupGrid.svelte";
  import EditMeetup from "./Meetups/EditMeetup.svelte";
  import Button from "./UI/Button.svelte";

  let meetups = [
    {
      id: "m1",
      title: "Coding bootcamp",
      subtitle: "Learn to code in 2 hours",
      description:
        "In this meetup, we have some experts that will teach you how to code",
      imageURL: "https://source.unsplash.com/daily",
      address: "27th Beck Road, MA",
      contactEmail: "code@test.com",
      isFavorite: false,
    },
    {
      id: "m2",
      title: "Swim Together",
      subtitle: "Let's go swimming!",
      description: "We will learn how to swim like dolphins",
      imageURL: "https://source.unsplash.com/weekly?water",
      address: "18 Main St, MA",
      contactEmail: "swimming@fish.com",
      isFavorite: false,
    },
  ];

  let editMode = null;

  function addMeetup(event) {
    const newMeetup = {
      id: Math.random().toString(),
      title: event.detail.title,
      subtitle: event.detail.subtitle,
      description: event.detail.description,
      imageURL: event.detail.imageURL,
      contactEmail: event.detail.email,
      address: event.detail.address,
    };

    meetups = [newMeetup, ...meetups];

    editMode = null;
  }

  function toggleFavorite(event) {
    const id = event.detail;
    const updatedMeetup = {
      ...meetups.find((m) => m.id === id),
    };

    updatedMeetup.isFavorite = !updatedMeetup.isFavorite;
    const meetupIndex = meetups.findIndex((m) => m.id === id);
    const updatedMeetups = [...meetups];
    updatedMeetups[meetupIndex] = updatedMeetup;
    meetups = updatedMeetups;
  }

  function cancelEdit() {
    editMode = null;
  }
</script>

<style>
  main {
    margin-top: 5rem;
  }

  .meetup-controls {
    margin: 1rem;
  }
</style>

<Header />

<main>
  <div class="meetup-controls">
    <Button
      on:click={() => {
        editMode = 'add';
      }}>
      New Meetup
    </Button>
  </div>
  {#if editMode === 'add'}
    <EditMeetup on:save={addMeetup} on:cancel={cancelEdit} />
  {/if}
  <MeetupGrid {meetups} on:togglefavorite={toggleFavorite} />
</main>
