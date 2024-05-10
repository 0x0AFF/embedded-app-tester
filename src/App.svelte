<script>
  import { GradientButton } from "flowbite-svelte";
  import { Label, Input } from "flowbite-svelte";
  import { Modal } from "flowbite-svelte";
  import { Card } from "flowbite-svelte";
  let defaultModal = false;
  let iframeSrc = "";
</script>

<Card>
  <div class="p-8">
    <div class="mb-6 flex flex-col items-start">
      <Label for="large-input" class="block mb-2">Enter page url</Label>
      <Input
        bind:value={iframeSrc}
        id="large-input"
        size="lg"
        placeholder="https://example.com"
      />
    </div>
    <GradientButton
      size="lg"
      class="w-full"
      on:click={() => (defaultModal = true)}
      color="purpleToBlue">Pay</GradientButton
    >
  </div>
</Card>
<Modal
  class="p-0 m-0 rounded-xl text-center h-95 top-0 right-0"
  size="sm"
  title="Оплата заказа"
  placement="center-right"
  bind:open={defaultModal}
  autoclose
>
  <iframe class="rounded-xl w-full h-full text-center" src={iframeSrc} title="paymentpage" />
</Modal>

