<script>
  import unsortedEntries from "$lib/data/entries.json";
  import tags from "$lib/data/tags.json";

  export const prerender = true;

  $: tagsMap = tags.reduce((obj, tag) => ({...obj, [tag.id]: tag.name}), {});

  const decodedTags = {
    // time
    "СС": "Время: < 09.2021",
    "С": "Время: 2021/2022",
    "С(Б)": "Время: лето 2022",
    "Н": "Время: 2022/2023",

    // authors
    "П: ПЛАТОН": "Автор: Платон",
    "П: А. НИКОЛАЕВИЧ": "Автор: Александр Николаевич",
    "П: ТИМОФЕЙ": "Автор: Тимофей Потапенко",
    "П: ЖЕНЯ АНИСОВЕЦ": "Автор: Женя Анисовец",
    "П: А. ИСААКОВИЧ": "Автор: Александр Исаакович",
    "П: НИКИТА": "Автор: Никита",
    "П: РОМА": "Автор: Рома",
    "П: ЛИЛИЯ": "Автор: Лилия Александровна",
    "П: ИГОРЬ": "Автор: Игорь",
    "П: ЛИЗА": "Автор: Лиза",

    // categories
    "К: НН": "Категория: НН",
    "К: ХА": "Категория: ХА",
    "К: СС": "Категория: СС",

    // awards
    "ЛГНД": "Легендарная",
    "БЦ: Ф": "Финалист",

    // other
    "НП": "Неприличные",
    "ГС": "Гостевая",
  };

  let showNp = false;
  let showTags = false;
  let singleColumn = false;

  $: entries = unsortedEntries
    .sort((a, b) => b.createdTime - a.createdTime)
    .filter(a => !a.archived && (showNp || !a.associatedTagIds.includes("MYQ1yWLCApC9mClL")));

  function processTags(tagIds) {
    return tagIds
      .map(id => decodedTags[tagsMap[id].toUpperCase().trim()])
      .sort();
  }
</script>

<svelte:head>
  <title>Битва Цитат</title>
</svelte:head>

<h1 class="font-bold text-4xl mb-4">Битва Цитат</h1>
<p class="text-lg mb-2">
  На этой странице представлены все цитаты, существующие на данный момент.
  Во время Битвы Цитат 2023 тут можно будет увидеть количество голосов и
  турнирную сетку. А сейчас вы можете посмотреть на все цитаты, которые
  будут участвовать или когда-то участвовали.
</p>
<p class="text-lg mb-2">
  Отправить свои цитаты можно сюда:
  <a class="text-amber-500 underline" href="https://t.me/sitting33">@sitting33</a>
</p>
<p class="text-lg mb-2">
  <a class="text-amber-500 underline" href="/tags">Узнать больше про теги</a>
</p>

<h2 class="font-bold text-2xl mt-6 mb-4">Цитаты</h2>
<div class="mb-4 p-3 backdrop-blur-md backdrop-brightness-50 rounded-xl accent-amber-500">
  <h3 class="font-bold text-xl mb-2">Настройки</h3>

  <div class="mb-1">
    <input type="checkbox" id="show-np" bind:checked={showNp}>
    <label for="show-np">Показать неприличные</label>
  </div>
  <div class="mb-1">
    <input type="checkbox" id="show-tags" bind:checked={showTags}>
    <label for="show-tags">Показать теги цитат</label>
  </div>
  <div>
    <input type="checkbox" id="single-col" bind:checked={singleColumn}>
    <label for="single-col">В одну колонку</label>
  </div>
</div>
<div class="grid gap-2" class:grid-cols-2={!singleColumn} class:grid-cols-1={singleColumn}>
  {#each entries as entry (entry.id)}
    <div class="p-3 text-md rounded-xl bg-[#200C01]">
      {#if showTags}
        <div class="mb-2 flex flex-wrap gap-1">
          {#each processTags(entry.associatedTagIds) as tag}
            <span class="text-sm py-0.5 px-1 bg-amber-950 rounded">{tag}</span>
          {/each}
        </div>
      {/if}
      <h3 class="font-medium mb-2">{entry.title}</h3>
      <p class="text-sm whitespace-pre-wrap">{entry.content}</p>
    </div>
  {/each}
</div>
