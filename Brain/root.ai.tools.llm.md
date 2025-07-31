#llm #tooling #embedding #multimodal
LLM is a utility that creates a level of abstraction over models. It can not onlyt creaztAe chats over arbitrary models but also create embeddings. Also uses a lot of pluygins which allows for multi8modality.

Example from article. The following embeds all the songs with CLAP:
```bash
llm embed-multi -m clap songs --files $MC '*'
```

More info: PagedOut! #6 - Foundation models and UNIX by Evangelos #Lamprou