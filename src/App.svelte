<script lang="ts">
  import dti from "dom-to-image";
  import "@melloware/coloris/dist/coloris.css";
  import Coloris from "@melloware/coloris";
  import "./app.css";
  let titleFontSize: number = 20;
  let descriptionFontSize: number = 16;
  let positonText: number = 0;
  let title: string = "ถ้าเกิดคุณง่วงคุณก็จะนอน";
  let description: string =
    "Lorem ipsum dolor sit amet consectetur adipisicing elit. Quam aut nisi beatae mollitia! Cum, odit nulla";
  let image;

  let titleColor: string = "#2563EB";
  let descriptionColor: string = "#fff";

  Coloris.init();

  function increaseFontSizeTitle() {
    titleFontSize += 2;
  }

  function decreaseFontSizeTitle() {
    titleFontSize -= 2;
  }

  function increaseFontSizeDescription() {
    descriptionFontSize += 2;
  }

  function decreaseFontSizeDescription() {
    descriptionFontSize -= 2;
  }

  function updatePositionText(target) {
    positonText = target.value;
  }

  function changeTitle(target) {
    title = target.value;
  }

  function changeDescription(target) {
    description = target.value;
  }

  async function download() {
    let dom = document.getElementById("content");
    try {
      let dataUrl = await dti.toPng(dom);
      var link = document.createElement("a");
      link.download = "meme.png";
      link.href = dataUrl;
      link.click();
    } catch (error) {
      console.log("your download got error", error);
    }
  }

  function upload(target) {
    let uploadedImage = target.files[0];
    let reader = new FileReader();
    reader.onload = () => {
      image = reader.result;
    };
    reader.readAsDataURL(uploadedImage);
    if (!image) {
      console.log("error uploaded");
    }
  }

  function updateTitleColor(target) {
    let color = target.value;
    target.style.backgroundColor = color;

    titleColor = color;
  }

  function updateDescriptionColor(target) {
    let color = target.value;
    target.style.backgroundColor = color;

    descriptionColor = color;
  }
</script>

<main
  class="p-12 min-h-screen grid place-content-center gap-4 scale-[0.6] sm:scale-75 md:scale-90 lg:scale-100"
>
  <div class="flex flex-col justify-content items-align">
    <h3 class="text-center text-4xl uppercase">Make Your Meme</h3>
    <div
      class="relative flex flex-col border-8 border-gray-600 rounded-lg "
      id="content"
    >
      <img
        class="w-full h-full object-cover"
        src={image || "/500x500.png"}
        alt="555"
      />
      <div class="absolute text-white mx-4" style={`bottom:${positonText}px`}>
        <p
          class="text-blue-600 font-bold"
          style={`font-size: ${titleFontSize}px; color: ${titleColor}`}
        >
          {title}
        </p>
        <p
          class="text-white"
          style={`font-size: ${descriptionFontSize}px;color: ${descriptionColor}`}
        >
          {description}
        </p>
      </div>
    </div>
    <div class="max-auto mt-5">
      <input
        type="text"
        class="border-2 border-gray-400 rounded-md h-10 w-full outline-none text-2xl"
        value={title}
        on:input={(e) => changeTitle(e.target)}
      />
    </div>
    <div class="max-auto mt-5">
      <textarea
        rows="5"
        class="border-2 border-gray-400 rounded-md h-50 w-full outline-none text-lg"
        value={description}
        on:input={(e) => changeDescription(e.target)}
      />
    </div>
    <div class="mx-auto mt-5">
      <label
        class="rounded-lg border-2 border-yellow-500 p-4 hover:bg-yellow-600 transition"
        for="upload"
      >
        Upload</label
      >
      <input
        style="display: none"
        id="upload"
        type="file"
        on:change={(e) => upload(e.target)}
      />
      <button
        class="rounded-lg border-2 border-blue-500 p-4 hover:bg-blue-600 transition"
        on:click={() => download()}>DownLoad</button
      >
    </div>
    <div class="mx-auto mt-2">
      <button class="btn-decrease" on:click={decreaseFontSizeTitle}>-</button>
      ขนาด Title
      <button class="btn-increase" on:click={increaseFontSizeTitle}>+</button>
    </div>
    <div class="mx-auto mt-2">
      <button class="btn-decrease" on:click={decreaseFontSizeDescription}
        >-</button
      >
      ขนาด Description
      <button class="btn-increase" on:click={increaseFontSizeDescription}
        >+</button
      >
    </div>
    <div class="mx-auto mt-2">
      ความสูง
      <input
        class="w-full h-2 bg-blue-100 appearance-none outline-none rounded"
        type="range"
        id="points"
        name="points"
        min="0"
        max="150"
        value={positonText}
        on:input={(e) => updatePositionText(e.target)}
      />
    </div>
    <div class="mx-auto mt-2">
      <p>สี หัวข้อ</p>
      <input
        class="description-color"
        data-coloris
        on:input={(e) => updateTitleColor(e.target)}
      />
    </div>
    <div class="mx-auto mt-2">
      <p>สี รายระเอียด</p>
      <input
        class="title-color"
        data-coloris
        on:input={(e) => updateDescriptionColor(e.target)}
      />
    </div>
  </div>
</main>

<style>
</style>
