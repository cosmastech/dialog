# Dialog

Humanized Conversation API (using LLM)

> conversations in a human way without exposing that it's a LLM answering

## Settings

To use this project, you need to have a `.csv` file with the knowledge base and a `.toml` file with your prompt configuration.

### `.csv` knowledge base

**fields:**

- category
- subcategory: used to customize the prompt for specific questions
- question
- content: used to generate the embedding

**example:**

```csv
category,subcategory,question,content
faq,promotions,loyalty-program,"The company XYZ has a loyalty program when you refer new customers you get a discount on your next purchase, ..."
```

To load the knowledge base into the database, make sure the database is up and then, inside `src` folder, run `make load-data path="../data/know.csv"` (or pass another path to you .csv).

### `.toml` prompt configuration

The `[prompt.header]`, `[prompt.suggested]`, and `[fallback.prompt]` fields are mandatory fields used for processing the conversation and connecting to the LLM.

The `[fallback.prompt]` field is used when the LLM does not find a compatible embedding on the database, without it, it would allucinate on possible answers for questions outside of the scope of the embeddings.

It is also possible to add information to the prompt for subcategories and chose some optional llm parameters like temperature (defaults to 0.2), see below for an example of a complete configuration:

```toml
[llm]
temperature = 0.2

[prompt]
header = """You are a service operator called Avelino from XYZ, you are an expert in providing
qualified service to high-end customers. Be brief in your answers, without being long-winded
and objective in your responses. Never say that you are a model (AI), always answer as Avelino.
Be polite and friendly!"""

suggested = "Here is some possible content that could help the user in a better way."

[prompt.subcategory.loyalty-program]

header = """The client is interested in the loyalty program, and needs to be responded to in a
salesy way; the loyalty program is our growth strategy."""

[fallback]
prompt = """I'm sorry, I didn't understand your question. Could you rephrase it?"""
```

### Environment Variables

Look at the [`.env.sample`](.env.sample) file to see the environment variables needed to run the project.

## Run the project

> we assume you are familiar with [Docker](https://www.docker.com/)

```bash
cp .env.sample .env # edit the .env file, add the OPENAI token and the path to the .csv and .toml files
docker compose up
```

After uploading the project, go to the documentation `http://localhost:8000/docs` to see the API documentation.

### Docker

The **dialog** docker image is distributed in [GitHub Container Registry](https://github.com/orgs/talkdai/packages/container/package/dialog) with the tag `latest`.

**image:** `docker pull ghcr.io/talkdai/dialog:latest`

### local development

We've used Python and bundled packages with `poetry`, now it's up to you - ⚠️ we're not yet at the point of explaining in depth how to develop and contribute, the [`Makefile`](Makefile) in root and the [Makefile](src/Makefile) in `src` folder may help you.
