
<p align="center">
    <img src="https://github.com/louis030195/screen-pipe/assets/25003283/289bbee7-79bb-4251-9516-878a1c40dcd0" width="200"/>
</p>

<p align="center">
    <a href="https://screenpi.pe" target="_blank">
        <img src="https://img.shields.io/badge/Join%20Waitlist-Desktop%20App-blue?style=for-the-badge" alt="Join Waitlist for Desktop App">
    </a>
</p>

<p align="center">
    <a href="https://www.bensbites.com/">
        <img src="https://img.shields.io/badge/Featured%20on-Ben's%20Bites-blue?style=flat-square" alt="Featured on Ben's Bites">
    </a>
    <a href="https://discord.gg/dU9EBuw7Uq">
        <img src="https://img.shields.io/discord/823813159592001537?color=5865F2&logo=discord&logoColor=white&style=flat-square" alt="Join us on Discord">
    </a>
        <a href="https://twitter.com/screen_pipe"><img alt="X account" src="https://img.shields.io/twitter/url/https/twitter.com/diffuserslib.svg?style=social&label=Follow%20%40screen_pipe"></a>
</p>

![Demo](./demo.gif)

# Extend your human memory with LLM. Open source, runs locally, developer friendly. 

> Civilization progresses by the number of operations it can perform without conscious effort.  
> — **Whitehead**

Library for devs to build AI apps on top of all your life data.
We are actuvely working  [to make it better](https://github.com/louis030195/screen-pipe/issues/6), make suggestions, post bugs, give feedback!

Chat with an AI that knows everything about you. Record your screens & audio 24/7. You own your data. Rust. 

## Getting started

Struggle to get it running? [I'll install it with you in a 15 min call.](https://cal.com/louis030195/screenpipe)

<details>
  <summary>MacOS</summary>

1. Install dependencies:
```bash
# On Mac
brew install ffmpeg
brew install jq
```

Install [Rust](https://www.rust-lang.org/tools/install).

2. Clone the repo:

```bash
git clone https://github.com/louis030195/screen-pipe
```

```bash
# This runs a local SQLite DB + an API + screenshot, ocr, mic, stt, mp4 encoding
cargo build --release --features metal # remove "--features metal" if you do not have M series processor

# sign the executable to avoid mac killing the process when it's running for too long
codesign --sign - --force --preserve-metadata=entitlements,requirements,flags,runtime ./target/release/screenpipe

# then run it
(cd screen-pipe/ && ./target/release/screenpipe) # add "--disable-audio" if you don't want audio to be recorded

# then run Vercel App
# set up you OPENAI API KEY
mkdir -p screen-pipe/examples/ts/vercel-ai-chatbot && echo "OPENAI_API_KEY=INSERT_YOUR_API_KEY_HERE" > screen-pipe/examples/ts/vercel-ai-chatbot/.env
# install dependencies and run local web server
(cd screen-pipe/examples/ts/vercel-ai-chatbot/ && npm install && npm run dev)


```
</details>

<details>
  <summary>Windows</summary>
  
  1. Install dependencies:

```bash
# Install ffmpeg (you may need to download and add it to your PATH manually)
# Visit https://www.ffmpeg.org/download.html for installation instructions
```

 Install [Rust](https://www.rust-lang.org/tools/install).

  2. Clone the repo:

```bash
git clone https://github.com/louis030195/screen-pipe
cd screen-pipe
```

  3. Run the API:

```bash
# This runs a local SQLite DB + an API + screenshot, ocr, mic, stt, mp4 encoding
cargo build --release --features cuda # remove "--features cuda" if you do not have a NVIDIA GPU

# then run it
./target/release/screenpipe
```
</details>

<details>
  <summary>Linux</summary>

```bash
sudo apt-get update
sudo apt-get install -y libavformat-dev libavfilter-dev libavdevice-dev ffmpeg libasound2-dev
curl -sSL https://raw.githubusercontent.com/louis030195/screen-pipe/main/install.sh | sh
```

  Now you should be able to `screenpipe`. (You may need to restart your terminal, or find the CLI in `$HOME/.local/bin`)
</details>


## Usage
<details>
<summary>
Check which tables you have in the local database</summary>
```bash
sqlite3 screen-pipe/data/db.sqlite ".tables" 
```
</details>
<details>
<summary>
Print a sample audio_transcriptions from the database</summary>
```bash
sqlite3 screen-pipe/data/db.sqlite ".mode json" ".once /dev/stdout" "SELECT * FROM audio_transcriptions ORDER BY id DESC LIMIT 1;" | jq .
```
</details>
<details>
<summary>
Print a sample frame_OCR_text from the database</summary>
```bash
sqlite3 screen-pipe/data/db.sqlite ".mode json" ".once /dev/stdout" "SELECT * FROM ocr_text ORDER BY frame_id DESC LIMIT 1;" | jq -r '.[0].text'
```
</details>
<details>
<summary>
Print a sample frame_recording from the database</summary>
```bash
ffplay "screen-pipe/data/2024-07-12 01:14:14.078958 UTC.mp4"
```
</details>
<details>
<summary>
Print a sample audio_recording from the database</summary>
```bash
ffplay "screen-pipe/data/Display 1 (output)_2024-07-12_01-14-11.mp3"
```
</details>

<details>
  <summary>Example to query the API</summary>
  
1. Basic search query
```bash
curl "http://localhost:3030/search?q=Neuralink&limit=5&offset=0&content_type=ocr" | jq
```
</details>
<details>
  <summary>Other Example to query the API</summary>

  ```bash
# 2. Search with content type filter (OCR)
curl "http://localhost:3030/search?q=QUERY_HERE&limit=5&offset=0&content_type=ocr"

# 3. Search with content type filter (Audio)
curl "http://localhost:3030/search?q=QUERY_HERE&limit=5&offset=0&content_type=audio"

# 4. Search with pagination
curl "http://localhost:3030/search?q=QUERY_HERE&limit=10&offset=20"

# 6. Search with no query (should return all results)
curl "http://localhost:3030/search?limit=5&offset=0"
  ```
</details>
<br><br>
Keep in mind that it's still experimental.
<br><br>
https://github.com/user-attachments/assets/95e343ab-f76a-4f8b-aa01-eca86d255005

## Use cases:

- RAG & question answering: Quickly find information you've forgotten or misplaced
- Automation: 
  - Automatically generate documentation
  - Populate CRM systems with relevant data
  - Synchronize company knowledge across platforms
  - Automate repetitive tasks based on screen content
- Analytics:
  - Track personal productivity metrics
  - Organize and analyze educational materials
  - Gain insights into areas for personal improvement
  - Analyze work patterns and optimize workflows
- Personal assistant:
  - Summarize lengthy documents or videos
  - Provide context-aware reminders and suggestions
  - Assist with research by aggregating relevant information
- Collaboration:
  - Share and annotate screen captures with team members
  - Create searchable archives of meetings and presentations
- Compliance and security:
  - Track what your employees are really up to
  - Monitor and log system activities for audit purposes
  - Detect potential security threats based on screen content

## Example vercel/ai-chatbot that query screenpipe autonomously

Check this example of screenpipe which is a chatbot that make requests to your data to answer your questions

https://github.com/louis030195/screen-pipe/assets/25003283/6a0d16f6-15fa-4b02-b3fe-f34479fdc45e

## Status 

Alpha: runs on my computer (`Macbook pro m3 32 GB ram`) 24/7.

- [x] screenshots
- [x] optimised screen & audio recording (mp4 & mp3 encoding, estimating 30 gb/m with default settings)
- [x] sqlite local db
- [x] OCR
- [x] audio + stt (works with multi input & output devices)
- [x] local api
- [ ] TS SDK
- [ ] multimodal embeddings
- [ ] cloud storage options (s3, pgsql, etc.)
- [ ] cloud computing options
- [ ] bug-free & stable
- [ ] custom storage settings: customizable capture settings (fps, resolution)
- [ ] data encryption options & higher security
- [ ] fast, optimised, energy-efficient modes


[Example with vercel/ai-chatbot project here inside the repo here:](https://github.com/louis030195/screen-pipe/tree/main/examples/ts/vercel-ai-chatbot)

## Why open source?

Recent breakthroughs in AI have shown that context is the final frontier. AI will soon be able to incorporate the context of an entire human life into its 'prompt', and the technologies that enable this kind of personalisation should be available to all developers to accelerate access to the next stage of our evolution.  

## Principles 

This is a library intended to stick to simple use case:
- record the screen & associated metadata (generated locally or in the cloud) and pipe it somewhere (local, cloud)

Think of this as an API that let's you do this:

```bash
screenpipe | ocr | llm "send what i see to my CRM" | api "send data to salesforce api"
```

Any interfaces are out of scope and should be built outside this repo, for example:
- UI to search on these files (like rewind)
- UI to spy on your employees
- etc.

## Contributing

Contributions are welcome! If you'd like to contribute, please read [CONTRIBUTING.md](CONTRIBUTING.md).

## Licensing

The code in this project is licensed under MIT license. See the [LICENSE](LICENSE.md) file for more information.

## Related projects

This is a very quick & dirty example of the end goal that works in a few lines of python:
https://github.com/louis030195/screen-to-crm

Very thankful for https://github.com/jasonjmcghee/xrem which was helpful. Although screenpipe is going in a different direction.

## FAQ

<details>
  <summary>What's the difference with adept.ai and rewind.ai?</summary>

  - adept.ai is a closed product, focused on automation while we are open and focused on enabling tooling & infra for a wide range of applications like adept 
  - rewind.ai is a closed product, focused on a single use case (they only focus on meetings now), not customisable, your data is owned by them, and not extendable by developers 

</details>

<details>
  <summary>Where is the data stored?</summary>
  
  - 100% of the data stay local in a SQLite database and mp4/mp3 files. You own your data
</details>

<details>
  <summary>How can I customize capture settings to reduce storage and energy usage?</summary>
  
  - You can adjust frame rates and resolution in the configuration. Lower values will reduce storage and energy consumption. We're working on making this more user-friendly in future updates.
</details>

<details>
  <summary>What are some practical use cases for screenpipe?</summary>
  
    - RAG & question answering
    - Automation (write code somewhere else while watching you coding, write docs, fill your CRM, sync company's knowledge, etc.)
    - Analytics (track human performance, education, become aware of how you can improve, etc.)
    - etc.
    - We're constantly exploring new use cases and welcome community input!
</details>

https://github.com/user-attachments/assets/edb503d4-6531-4527-9b05-0397fd8b5976
