<script>
  import { Accordion, AccordionItem, GradientButton, Checkbox } from "flowbite-svelte";
  import { Label, Input } from "flowbite-svelte";
  import { Modal } from "flowbite-svelte";
  import { Card } from "flowbite-svelte";
  let defaultModal = false;
  let iframeSrc = "";
  let modalWidth = '500px';
  let modalHeight = '478px';

  let messageModal = false;
  let messageContent = {};
  let isSbermarket = true

  const handlePostMessage = (event) => {
    try {
      const result = JSON.parse(event.data)
      if (result.type === 'PAGE_DIMENSIONS') {
        messageContent = result.payload;
        messageModal = true;
        window.removeEventListener('message', handlePostMessage);
      };
    } catch(error) {
      throw new Error(error)
    }
  };

  const clickHandler = () => {
    if (isSbermarket) {
      window.addEventListener('message', handlePostMessage);
    }
    defaultModal = true
  }
</script>

<Card>
  <div class="p-8">
    <div class="mb-6 flex flex-col items-start">
      <Label for="large-input" class="block mb-2">Enter paymentpage url</Label>
      <Input
        bind:value={iframeSrc}
        id="large-input"
        size="lg"
        placeholder="https://example.com"
      />
      <Checkbox class="mt-10" bind:checked={isSbermarket}>Сбермаркет</Checkbox>
      <Accordion title="enter styles" class="mt-10">
        <AccordionItem open>
          <span slot="header">STYLES</span>
          <Label for="width-input" class="block mb-2 text-left">Enter modal width</Label>
          <Input
          bind:value={modalWidth}
          id="width-input"
          size="sm"
          placeholder="Enter modal window width"
        />
        <Label for="height-input" class="block mb-2 text-left mt-5">Enter modal height</Label>
        <Input
          bind:value={modalHeight}
          id="height-input"
          size="sm"
          placeholder="Enter modal window height"
        />
        </AccordionItem>
      </Accordion>
    </div>
    <GradientButton
      size="lg"
      class="w-full"
      on:click={clickHandler}
      color="purpleToBlue">Pay</GradientButton
    >
  </div>
</Card>
<Modal
  class='p-0 m-0 rounded-xl w-[var(--modal-width)] h-[var(--modal-height)] text-center top-0 right-0"'
  style={`--modal-width:${modalWidth}; --modal-height:${modalHeight};`}
  placement="center-right"
  bind:open={defaultModal}
  autoclose
>
  <iframe class="rounded-xl w-full h-full text-center" src={iframeSrc} title="paymentpage" />
</Modal>
<Modal
  class='p-0 m-0 rounded-xl w-[1000px] h-[200px] text-center top-0 right-0"'
  placement="top-left"
  bind:open={messageModal}
  autoclose
>
  <h3>RECEIVED POST MESSAGE</h3>
  <p>Width is: <b>{messageContent.width}</b></p>
  <p>Height is: <b>{messageContent.height}</b></p>
</Modal>
