# 🎥 YouTube Automático Leonardo AI

#### Vídeo: https://youtu.be/ZoJc8OJjS_s

## 📝 Prompts

### TABELA HISTÓRIAS - Fonte

O FBI e o Departamento de Polícia de Los Angeles estão investigando um dos maiores roubos de dinheiro na história da cidade, após o furto de até $30 milhões de um depósito de dinheiro no Vale de San Fernando, informou uma fonte da aplicação da lei familiarizada com a investigação à CNN na quinta-feira.

O roubo aconteceu na noite do Domingo de Páscoa em uma instalação não identificada em Sylmar, um bairro suburbano no Vale de San Fernando, onde dinheiro de empresas de toda a região é manuseado e armazenado, disse a fonte.

O Los Angeles Times foi o primeiro a reportar o incidente.

O escritório do FBI em Los Angeles disse à CNN na quinta-feira que estão investigando "um grande roubo" no Vale de San Fernando e estão trabalhando em conjunto com o LAPD, mas não compartilharam mais detalhes citando uma investigação em andamento. O LAPD encaminhou todos os comentários ao FBI.

Os ladrões conseguiram acessar o prédio e entrar no cofre sem disparar os alarmes, e os investigadores acreditam que se trata de um grupo sofisticado com base em sua habilidade de evitar a detecção, disse a fonte.

Uma área de foco para a investigação é se o grupo tinha conhecimento interno da instalação, disse a fonte, que acrescentou que o roubo foi descoberto na segunda-feira.

A instalação era operada pela empresa de segurança privada GardaWorld, informou a fonte. A CNN entrou em contato com a empresa para comentar sobre o incidente.

Segundo o Los Angeles Times, anteriormente o maior roubo de dinheiro na cidade ocorreu em 12 de setembro de 1997, quando $18,9 milhões foram roubados do antigo local da Dunbar Armored Inc. na Rua Mateo. Os suspeitos foram eventualmente capturados.

### TABELA TIPOS DE HISTÓRIA – Prompt

Leia o artigo abaixo.

Você é um repórter de notícias de tecnologia.

Vamos criar uma revisão factual de 200 palavras do artigo.

Vou enviar essa narração para um gerador de voz de IA que será usado sobre imagens.

Primeiro, crie um título de 10 palavras.

Segundo, crie uma introdução de uma única frase que atraia entusiastas de tecnologia.

Depois, escreva uma frase explicando por que este artigo é importante.

Em seguida, fale sobre os fatos do artigo.

Finalize dizendo ao leitor para tomar uma ação para aproveitar as notícias compartilhadas no artigo.

### TABELA TIPOS DE HISTÓRIA – Cenas Prompts JSON

Eu tenho uma história para a qual preciso gerar prompts para geração de imagens com IA. Cada prompt deve ser projetado como uma ilustração dinâmica e vívida de quadrinhos. Os prompts devem incluir uma descrição geral, ações dos personagens, aparências dos personagens e o cenário/fundo, adaptados a cada sentença da história.

Por favor, formate os resultados como um objeto JSON contendo um array chamado "pairs". Cada objeto dentro do array deve ter duas chaves: "original", com a sentença original da história, e "aiImagePrompts", que contém um array de três strings. Cada string deve ser um prompt detalhado para gerar uma imagem no estilo de quadrinhos, focando no conteúdo narrativo da sentença correspondente.

Diretrizes para os prompts:

Declaração de Estilo de Quadrinhos: Comece cada prompt com "Crie uma ilustração dinâmica e vívida de quadrinhos de..."
Descrição dos Personagens e Ações: Descreva claramente os personagens e suas ações conforme retratados na cena.
Cenário/Fundo: Inclua detalhes do ambiente ou fundo onde a ação ocorre.
Humor e Tema: Reflita o humor e os elementos temáticos da sentença nos detalhes da ilustração.
Elementos Visuais: Incorpore elementos visuais específicos que devem ser destacados, como tecnologia, emoções específicas ou características ambientais únicas.
O objetivo é receber prompts criativos e envolventes que inspirem ilustrações detalhadas e vivas, encapsulando a essência da história através da lente da arte dos quadrinhos.

Aqui está a história que quero usar:

## TABELA TIPOS DE HISTÓRIA - Model ID

aa77f04e-3eec-4034-9c07-d0f619684628

## URLs

Gerar imagem Leonardo AI - https://cloud.leonardo.ai/api/rest/v1/generations
Buscar imagem Leonardo AI - https://cloud.leonardo.ai/api/rest/v1/generations/[INSERIR_GENERATION_ID]
Gerar vídeo JSON2Video - https://api.json2video.com/v2/movies
Gerar áudio Elevenlabs - https://api.elevenlabs.io/v1/text-to-speech/[INSERIR_VOICE_ID]

## Fórmulas

### Tabela Histórias

```bash
lookup('Tipos de História', 'Prompt')

lookup('Tipos de História', 'Largura Output')

lookup('Tipos de História', 'Altura Output')

lookup('Tipos de História', 'Largura Imagem')

lookup('Tipos de História', 'Altura Imagem')

lookup('Tipos de História', 'Cenas Prompts JSON')
```

## 👨‍💻 Códigos para geração da História e Cenas

### Preparar prompt node

```bash
const data = items[0].json;

const fonte = data["Fonte"];
const promptTipoHistoria = data["Prompt"][0].value;

return [
  {
    json: {
      fonte: fonte,
      prompt: promptTipoHistoria
    }
  }
];
```

### Organizar dados node

```bash
const openaiData = items[0].json.message.content;
const baserowData = items[1].json;

const combineContentValues = (data) => {
  return Object.values(data).flatMap(value => {
    if (Array.isArray(value)) {
      return value.join(", ");
    } else if (typeof value === 'object' && value !== null) {
      return [];
    } else {
      return value;
    }
  }).join("\n");
};

const combinedOutput = combineContentValues(openaiData);

return [
  {
    json: {
      id: baserowData.id,
      combinedOutput: combinedOutput.trim()
    }
  }
];
```

### Preparar prompt node 2

```bash
const mergedData = items;

const data1 = mergedData[0].json;
const data2 = mergedData.length > 1 ? mergedData[1].json : {};

const cenasPromptsJsonString = data1["Cenas Prompts JSON"] || "";
const historiaArray = data1["Histórias"] || [];

const cenasPromptsJson = cenasPromptsJsonString.split("\n").join("\n");
const historia = historiaArray.map(item => item.value).join("\n");

const prompt = `
  ${cenasPromptsJson}

  ${historia}
`;

return [
  {
    json: {
      prompt: prompt.trim()
    }
  }
];
```

### Transformar array (\*Converter array para lista)

```bash
const pairs = items[0].json.message.content.pairs;
const result = [];

for (const pair of pairs) {
  for (const prompt of pair.aiImagePrompts) {
    result.push({ json: { aiImagePrompt: prompt, original: pair.original } });
  }
}

return result;
```

## 👨‍💻 Códigos para geração de Imagem e Vídeo

### Transformar array (\*Converter array para lista 2)

```bash
return items
  .map(item => {
    return {
      json: {
        "prompt": item.json['AI Image Prompts']
      }
    };
  });
```

### Extrair dimensão da imagem + model ID

```bash
const inputData = items[0].json;

const outputData = {
  width: inputData["Largura Imagem"],
  height: inputData["Altura Imagem"],
  modelId: inputData["Model ID"],
  prompt: inputData["Prompt"]
};

return [{ json: outputData }];
```

### Mesclar outputs por delay do merge

```bash
const inputData = items[0].json;

return items.slice(1).map(item => {
  return {
    json: {
      prompt: inputData.prompt,
      width: parseInt(inputData.width),
      height: parseInt(inputData.height),
      modelId: inputData.modelId
    }
  };
});
```

### Extrair AI Image Prompts

```bash
const items = $input.all();

const seenIds = new Set();
const uniquePrompts = [];

for (const item of items) {
    const data = item.json;
    if (!seenIds.has(data.id)) {
        seenIds.add(data.id);
        uniquePrompts.push({
            id: data.id,
            'AI Image Prompts': data['AI Image Prompts']
        });
    }
}

return uniquePrompts.map(prompt => ({ json: prompt }));
```

### Mesclar outputs por delay do Merge3

```bash
const items = $input.all();

const prompts = items.filter(item => item.json.id !== undefined);
const properties = items.filter(item => item.json.width !== undefined);

const mergedResults = prompts.map((prompt, index) => {
    const property = properties[index % properties.length];
    return {
        json: {
            ...prompt.json,
            width: property.json.width,
            height: property.json.height,
            modelId: property.json.modelId,
        }
    };
});

return mergedResults;
```

### Filtrar ID linha + ID Geração

```bash
const items = $input.all();

let baserowItems = [];
let generationItems = [];

for (let i = 0; i < items.length; i++) {
    let item = items[i].json;

    if (item.sdGenerationJob) {
        generationItems.push(item);
    } else {
        baserowItems.push(item);
    }
}

let output = [];

for (let i = 0; i < baserowItems.length; i++) {
    let baserowItem = baserowItems[i];

    if (generationItems[i] && generationItems[i].sdGenerationJob) {
        let generationId = generationItems[i].sdGenerationJob.generationId;
        let id = baserowItem.id;

        output.push({ id, generationId });
    }
}

return output;
```

### Extrair ID Geração

```bash
const inputData = items.map(item => item.json);

const uniqueIDs = new Set();

inputData.forEach(item => {
  uniqueIDs.add(item["ID Geração"]);
});

const uniqueIDArray = Array.from(uniqueIDs);

return uniqueIDArray.map(id => ({ "ID Geração": id }));
```

### Identificar URL por ID da linha

```bash
const results = [];
const baserowData = [];
const imageData = [];

for (const item of items) {
  if (item.json.generations_by_pk) {
    imageData.push(item.json);
  } else {
    baserowData.push(item.json);
  }
}

for (const imageItem of imageData) {
  const generation = imageItem.generations_by_pk;
  if (generation && generation.generated_images.length > 0) {
    const imageUrl = generation.generated_images[0].url;
    const generationId = generation.id;

    const originalItem = baserowData.find(row => row['ID Geração'] === generationId);

    if (originalItem) {
      const rowId = originalItem.id;

      results.push({
        json: {
          row_id: rowId,
          image_url: imageUrl
        }
      });
    }
  }
}

return results;
```

### Extrair ID linha + URL imagem

```bash
const inputData = $input.all();

const uniqueItems = [];
const seenIds = new Set();

for (const item of inputData) {
  const id = item.json.id;
  const imagem = item.json.Imagem;

  if (!seenIds.has(id)) {
    seenIds.add(id);
    uniqueItems.push({ id, imagem });
  }
}


return uniqueItems;
```

### Extrair altura e largura output

```bash
const inputData = $input.all();

if (inputData.length > 0) {
  const firstItem = inputData[0].json;

  const larguraOutput = firstItem["Largura Output"] && firstItem["Largura Output"].length > 0
    ? firstItem["Largura Output"][0].value
    : null;

  const alturaOutput = firstItem["Altura Output"] && firstItem["Altura Output"].length > 0
    ? firstItem["Altura Output"][0].value
    : null;

  return [
    {
      larguraOutput,
      alturaOutput
    }
  ];
} else {
  return [
    {
      larguraOutput: null,
      alturaOutput: null
    }
  ];
}
```

### Preparar input para JSON2Video

```bash
let elementId = 1;
const processedLinks = new Set();
let elements = [];

let validLarguraOutput = null;
let validAlturaOutput = null;

items.forEach(item => {
  const larguraOutput = item.json["larguraOutput"];
  const alturaOutput = item.json["alturaOutput"];

  if (larguraOutput && alturaOutput) {
    validLarguraOutput = parseInt(larguraOutput, 10);
    validAlturaOutput = parseInt(alturaOutput, 10);
  }
});

items.forEach(item => {
  const id = item.json["id"];
  const imagem = item.json["imagem"];

  if (id && imagem) {
    let larguraOutput = validLarguraOutput;
    let alturaOutput = validAlturaOutput;

    if (!processedLinks.has(imagem) && larguraOutput && alturaOutput) {
      processedLinks.add(imagem);
      elements.push({
        id: elementId++,
        type: "image",
        src: imagem,
        duration: 2,
        zoom: 2,
        width: larguraOutput,
        height: alturaOutput,
        position: "center-center"
      });
    }
  }
});

let videos = [];
for (let i = 0; i < elements.length; i += 3) {
  const scenes = [];
  for (let j = 0; j < 3 && i + j < elements.length; j++) {
    scenes.push({
      comment: `Scene #${Math.floor(i / 3) + 1}`,
      elements: [elements[i + j]]
    });
  }

  if (scenes.length > 0) {
    const videoJson = {
      resolution: "full-hd",
      quality: "high",
      scenes: scenes
    };
    videos.push({ json: videoJson });
  }
}

return videos;
```

## 👨‍💻 Códigos para geração de Áudio

### Extrair conteúdo da coluna Cena

```bash
const uniqueScenes = {};
const data = items;

data.forEach(item => {
  const scene = item.json;
  if (!uniqueScenes[scene.Cenas]) {
    uniqueScenes[scene.Cenas] = scene.Cenas;
  }
});

const uniqueScenesArray = Object.values(uniqueScenes);

return uniqueScenesArray.map(scene => ({ json: { Cena: scene } }));
```

### Extrair ID coluna + URL áudio

```bash
const items = $input.all();

const videoItems = items.filter(item => item.json.hasOwnProperty('ID Vídeo'));
const audioItem = items.find(item => item.json.mimeType === 'audio/mpeg');

let videoItemToUpdate = null;

for (let video of videoItems) {
  if (!video.json.Áudio) {
    if (videoItemToUpdate === null || video.json.id < videoItemToUpdate.json.id) {
      videoItemToUpdate = video;
    }
  }
}

let result = {};

if (videoItemToUpdate) {
  videoItemToUpdate.json.Áudio = audioItem.json.webContentLink;
  result = {
    id: videoItemToUpdate.json.id,
    Áudio: videoItemToUpdate.json.Áudio
  };
}

return [result];
```

## 👨‍💻 Códigos para Áudio + Vídeo

### Extrair id + link Vídeo e Áudio

```bash
const inputData = items;

let filteredData = inputData.map(item => ({
  id: item.json.id,
  video: item.json.Vídeo,
  audio: item.json.Áudio
}));

let uniqueData = [];
let ids = new Set();

filteredData.forEach(item => {
  if (!ids.has(item.id)) {
    uniqueData.push(item);
    ids.add(item.id);
  }
});

return uniqueData.map(data => ({ json: data }));
```

### Extrair dimensão da imagem 2

```bash
const inputData = items[0].json;

const outputData = {
  width: inputData["Largura Output"],
  height: inputData["Altura Output"]
};

return [{ json: outputData }];
```

### Preparar input para JSON2Video 2

```bash
const items = $input.all();

const { width, height } = items[0].json;

const videoRequests = [];

items.slice(1).forEach((item, index) => {
  const scenes = [
    {
      comment: `Scene #${index + 1}`,
      elements: [
        {
          type: 'video',
          src: item.json.video,
          duration: -2
        }
      ],
    }
  ];

  const audioElements = [
    {
      type: 'audio',
      src: item.json.audio,
      duration: -1
    }
  ];

  const requestPayload = {
    id: 'Vídeo + Áudio',
    fps: 25,
    cache: false,
    draft: false,
    width: parseInt(width, 10),
    height: parseInt(height, 10),
    scenes: scenes,
    quality: 'high',
    elements: audioElements,
    settings: {},
    resolution: 'custom'
  };

  videoRequests.push({ json: requestPayload });
});

return videoRequests;
```

### Extrair ID linha

```bash
const items = $input.all();

const uniqueIds = new Set();

items.forEach((item) => {
  if (item.json && typeof item.json === 'object' && item.json.id) {
    uniqueIds.add(item.json.id);
  }
});

const uniqueIdsArray = Array.from(uniqueIds);

return uniqueIdsArray.map(id => ({ json: { id } }));
```

### Mesclar outputs por delay do merge 2

```bash
function mergeOutputs(inputs) {
    let mergedOutput = [];
    let ids = inputs.filter(input => input.json.id !== undefined);
    let movies = inputs.filter(input => input.json.movie !== undefined);

    ids.forEach((id, index) => {
        if (index < movies.length) {
            mergedOutput.push({
                ...id.json,
                ...movies[index].json
            });
        } else {
            mergedOutput.push(id.json);
        }
    });

    movies.slice(ids.length).forEach(movie => {
        mergedOutput.push(movie.json);
    });

    return mergedOutput;
}

let inputs = $input.all();

if (!inputs || inputs.length === 0) {
    throw new Error("No inputs received.");
}

let output = mergeOutputs(inputs);

return output;
```

## 👨‍💻 Códigos para Vídeo + Legenda

### Extrair ID e Áudio + Vídeo

```bash
function processInput(items) {
  let seenIds = new Set();

  let result = [];

  if (Array.isArray(items)) {
    items.forEach(item => {
      let currentItem = item.json || {};

      if (currentItem.id && currentItem["Vídeo + Áudio"]) {
        if (!seenIds.has(currentItem.id)) {
          seenIds.add(currentItem.id);
          result.push({ id: currentItem.id, "Vídeo + Áudio": currentItem["Vídeo + Áudio"] });
        }
      }
    });
  }
  return result;
}

let items = $input.all();

let output = processInput(items);

return output;
```

### Extrair altura e largura output 2

```bash
const inputData = $input.all();

if (inputData.length > 0) {
  const firstItem = inputData[0].json;

  const larguraOutput = firstItem["Largura Output"] && firstItem["Largura Output"].length > 0
    ? firstItem["Largura Output"][0].value
    : null;

  const alturaOutput = firstItem["Altura Output"] && firstItem["Altura Output"].length > 0
    ? firstItem["Altura Output"][0].value
    : null;

  return [
    {
      larguraOutput,
      alturaOutput
    }
  ];
} else {
  return [
    {
      larguraOutput: null,
      alturaOutput: null
    }
  ];
}
```

### Preparar input para JSON2Video 3

```bash
let elementId = 1;
const processedLinks = new Set();
let elements = [];
let validLarguraOutput = null;
let validAlturaOutput = null;

items.forEach(item => {
  const larguraOutput = item.json["larguraOutput"];
  const alturaOutput = item.json["alturaOutput"];

  if (larguraOutput && alturaOutput) {
    validLarguraOutput = parseInt(larguraOutput, 10);
    validAlturaOutput = parseInt(alturaOutput, 10);
  }
});

items.forEach(item => {
  const id = item.json["id"];
  const videoUrl = item.json["Vídeo + Áudio"];

  if (id && videoUrl) {
    let larguraOutput = validLarguraOutput;
    let alturaOutput = validAlturaOutput;

    if (!processedLinks.has(videoUrl) && larguraOutput && alturaOutput) {
      processedLinks.add(videoUrl);
      elements.push({
        id: elementId++,
        type: "video",
        src: videoUrl,
        duration: -1,
        zoom: 0,
        width: larguraOutput,
        height: alturaOutput,
        position: "center-center"
      });
    }
  }
});

let scenes = [];
elements.forEach(element => {
  scenes.push({
    elements: [element]
  });
});

const outputJson = {
  id: "Vídeo Completo",
  fps: 25,
  cache: false,
  draft: false,
  width: validLarguraOutput,
  height: validAlturaOutput,
  scenes: scenes,
  quality: "high",
  elements: [
    {
      type: "subtitles",
      language: "pt-BR",
      settings: {
        "all-caps": true,
        position: "mid-bottom-center",
        "font-size": 75,
        "font-family": "Luckiest Guy",
        "outline-width": 5
      }
    }
  ],
  settings: {},
  resolution: "custom"
};

return [{ json: outputJson }];
```

### Extrair ID linha 2

```bash
const items = $input.all();

const ids = items.map(item => item.json.id);

const uniqueIds = [...new Set(ids)];

return uniqueIds.map(id => ({ json: { id } }));
```

### Mesclar outputs por delay do merge 3

```bash
function mergeOutputs(inputs) {
    let mergedOutput = [];
    let ids = inputs.filter(input => input.json.id !== undefined);
    let movies = inputs.filter(input => input.json.movie !== undefined);

    ids.forEach((id, index) => {
        if (index < movies.length) {
            mergedOutput.push({
                ...id.json,
                ...movies[index].json
            });
        } else {
            mergedOutput.push(id.json);
        }
    });

    movies.slice(ids.length).forEach(movie => {
        mergedOutput.push(movie.json);
    });

    return mergedOutput;
}

let inputs = $input.all();

if (!inputs || inputs.length === 0) {
    throw new Error("No inputs received.");
}

let output = mergeOutputs(inputs);

return output;
```

---

Made with 💙 by eduardocodes 👋
