# screen-pipe

## Overview
Turn your screen into actions (using LLMs). Inspired by `adept.ai`, `rewind.ai`, `Apple Shortcut`. Rust + WASM.

## Screen to action using LLMs
Here's an example of server-side code written in TypeScript that takes the streamed data from ScreenPipe and uses a Large Language Model like OpenAI's to process text and images for analyzing sales conversations:

```typescript
import { ScreenPipe } from "screenpipe";
import { generateObject } from 'ai';
import { z } from 'zod';

const screenPipe = new ScreenPipe();

export async function onTick() {
  const data = await screenPipe.tick([1], {frames: 60}); // or screen [1, 2, 3, ...]
  // [{frame: [...], text: [...], metadata: [...]}, ...]

  const { object } = await generateObject({
    model: openai("gpt4-o"),
    schema: z.object({
      leads: z.array(z.object({
        name: z.string(),
        company: z.string(),
        role: z.string(),
        status: z.string(),
        messages: z.array(z.string()),
      }),
    })),
    prompt: "Fill salesforce CRM based on Bob's sales activity (this is what appeared on his screen): " +
     data.map((frame) => frame.text).join("\n"),
  });

  // Add to Salesforce API ...
}
```

## On your computer
To start capturing screen data and send it to a specific storage location such as Amazon S3, use the following command line interface (CLI) command:

```bash
screenpipe --path ./second-memory
```


## Features
- **Multi-Screen Support**: Capture and aggregate data from multiple screens simultaneously.
- **Video Recording**: Record continuous or event-triggered screen activities.
- **OCR Capabilities**: Extract text from captured frames or videos for further analysis.
- **Metadata Extraction**: Collect and store metadata related to screen activities for enhanced insights.
- **Flexible Storage Options**: Configure storage on local drives, cloud storage, or custom solutions tailored to enterprise needs.
- **Cross-Platform**: Runs smoothly on various operating systems thanks to its Rust base and WASM compilation.

## Installation

To install ScreenPipe, run the following command in your terminal:

```bash
curl -sL https://raw.githubusercontent.com/louis030195/screen-pipe/main/install.sh | sh
```


## Documentation

For more detailed information about the API and advanced configurations, please refer to the [ScreenPipe Documentation](https://github.com/yourusername/screenpipe/docs).

## Contributing

Contributions are welcome! If you'd like to contribute, please fork the repository and use a feature branch. Pull requests are warmly welcome.

## Licensing

The code in this project is licensed under MIT license. See the [LICENSE](LICENSE.md) file for more information.


