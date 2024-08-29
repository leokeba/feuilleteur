<script lang="ts">
  import Button, { Label } from '@smui/button';
  import Card, {
    Content,
    PrimaryAction,
    Actions,
    ActionButtons,
    ActionIcons,
  } from '@smui/card';
  import Paper from '@smui/paper';
  import Slider from '@smui/slider';
  import FormField from '@smui/form-field';

  let value = 50;
</script>

<svelte:head>
  <title>Feuilleteur</title>
</svelte:head>

<section>
    <div class="card-container" style="width: auto;">
        <Card>
            <PrimaryAction on:click={() => console.log("hello")} padded>
                Select .epub file to upload
            </PrimaryAction>

            <FormField align="end" style="display: flex;">
                <Slider style="flex-grow: 1;" bind:value />
                <span
                slot="label"
                style="padding: 12px; width: max-content; display: block;"
                >
                Amount of Wonder
                </span>
            </FormField>
            <ActionButtons>
                <Button class="button">Upload</Button>
            </ActionButtons>
        </Card>
    </div>
</section>