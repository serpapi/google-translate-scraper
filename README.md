# Google Translate Scraper

Google Translate Scraper translates text through a simple API and detects the source language. Depending on the text, responses also include sentence pairs, pronunciations, gender-specific translations, spelling corrections, and dictionary data such as alternative translations, definitions, and examples.

Choose structured JSON for applications or Markdown for LLMs and AI agents.

## How to scrape Google Translate

Translate any text with a simple GET request:

```text
https://serpapi.com/search?engine=google_translate&text=I+love+coffee.+It+helps+me+start+my+day.&from=auto&to=es&api_key=YOUR_SERPAPI_API_KEY
```

- `text` is the text to translate, up to 5,000 characters. Emojis and some rare characters count as two.
- `from` is the language of the text, such as `en`, or `auto` to detect it.
- `to` is the language to translate into, such as `es`.

See the [Google Translate languages](https://serpapi.com/google-translate-languages?utm_source=github_google_translate_scraper) page for all supported language codes.

[Register at SerpApi](https://serpapi.com?utm_source=github_google_translate_scraper) to get your API key, and keep it private. Try the request live in the [SerpApi Playground](https://serpapi.com/playground?engine=google_translate&text=I+love+coffee.+It+helps+me+start+my+day.&from=auto&to=es&utm_source=github_google_translate_scraper).

## Code examples

The code examples read your API key from the `SERPAPI_API_KEY` environment variable:

```bash
export SERPAPI_API_KEY="YOUR_SERPAPI_API_KEY"
```

Each example detects the language of “I love coffee. It helps me start my day.” and translates it into Spanish. The output looks like this:

```text
Me encanta el café. Me ayuda a comenzar mi día.
```

### curl integration

```bash
curl --get https://serpapi.com/search \
 --data-urlencode engine="google_translate" \
 --data-urlencode text="I love coffee. It helps me start my day." \
 --data-urlencode from="auto" \
 --data-urlencode to="es" \
 --data-urlencode api_key="$SERPAPI_API_KEY"
```

### JavaScript integration

Install the [SerpApi JavaScript library](https://github.com/serpapi/serpapi-javascript):

```bash
npm install serpapi
```

Create an `index.mjs` file:

```javascript
import { getJson } from "serpapi";

const results = await getJson({
  engine: "google_translate",
  api_key: process.env.SERPAPI_API_KEY,
  text: "I love coffee. It helps me start my day.",
  from: "auto",
  to: "es",
});

console.log(results.translation.target.text);
```

Run it with `node index.mjs`.

### Python integration

Install the [SerpApi Python library](https://github.com/serpapi/serpapi-python):

```bash
pip install serpapi
```

Create a `main.py` file:

```python
import os
import serpapi

client = serpapi.Client(api_key=os.getenv("SERPAPI_API_KEY"))
results = client.search({
    "engine": "google_translate",
    "text": "I love coffee. It helps me start my day.",
    "from": "auto",
    "to": "es",
})

print(results["translation"]["target"]["text"])
```

Run it with `python main.py`.

### Ruby integration

Install the [SerpApi Ruby library](https://github.com/serpapi/serpapi-ruby):

```bash
gem install serpapi
```

Create a `main.rb` file:

```ruby
require "serpapi"

client = SerpApi::Client.new(engine: "google_translate", api_key: ENV.fetch("SERPAPI_API_KEY"))
results = client.search(text: "I love coffee. It helps me start my day.", from: "auto", to: "es")

puts results[:translation][:target][:text]

client.close
```

Run it with `ruby main.rb`.

### PHP integration

Install the [SerpApi PHP library](https://github.com/serpapi/serpapi-php):

```bash
composer require serpapi/serpapi-php
```

Create an `index.php` file:

```php
<?php
require 'vendor/autoload.php';

use SerpApi\Client;

$client = new Client(getenv('SERPAPI_API_KEY'), 'google_translate');
$results = $client->search([
  'text' => 'I love coffee. It helps me start my day.',
  'from' => 'auto',
  'to' => 'es',
]);

echo $results->translation->target->text . PHP_EOL;
```

Run it with `php index.php`.

### Go integration

Create a Go module and install the [SerpApi Go library](https://github.com/serpapi/serpapi-golang):

```bash
go mod init example.com/translate
go get github.com/serpapi/serpapi-golang
```

Create a `main.go` file:

```go
package main

import (
	"fmt"
	"os"

	"github.com/serpapi/serpapi-golang"
)

func main() {
	setting := serpapi.NewSerpApiClientSetting(os.Getenv("SERPAPI_API_KEY"))
	setting.Engine = "google_translate"
	client := serpapi.NewClient(setting)

	results, err := client.Search(map[string]string{
		"text": "I love coffee. It helps me start my day.",
		"from": "auto",
		"to":   "es",
	})
	if err != nil {
		panic(err)
	}

	translation := results["translation"].(map[string]interface{})
	target := translation["target"].(map[string]interface{})
	fmt.Println(target["text"])
}
```

Run it with `go run .`.

### Rust integration

Create a Cargo project and add the [SerpApi Rust library](https://github.com/serpapi/serpapi-rust) and Tokio:

```bash
cargo new translate && cd translate
cargo add serpapi tokio --features tokio/full
```

Replace `src/main.rs` with:

```rust
use serpapi::serpapi::Client;
use std::collections::HashMap;
use std::env;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let mut default = HashMap::new();
    default.insert("api_key".to_string(), env::var("SERPAPI_API_KEY")?);
    default.insert("engine".to_string(), "google_translate".to_string());
    let client = Client::new(default)?;

    let mut parameter = HashMap::new();
    parameter.insert("text".to_string(), "I love coffee. It helps me start my day.".to_string());
    parameter.insert("from".to_string(), "auto".to_string());
    parameter.insert("to".to_string(), "es".to_string());

    let results = client.search(parameter).await?;
    println!("{}", results["translation"]["target"]["text"].as_str().unwrap_or_default());
    Ok(())
}
```

Run it with `cargo run`.

### Other programming languages

Official libraries are also available for [Java](https://github.com/serpapi/serpapi-java), [.NET](https://github.com/serpapi/serpapi-dotnet), [Swift](https://github.com/serpapi/serpapi-swift), and [C++](https://github.com/serpapi/serpapi-cpp). See [SerpApi integrations](https://serpapi.com/integrations?utm_source=github_google_translate_scraper) for setup guides, or use a simple GET request from any programming language.

### Errors

Failed requests return an HTTP error status and a JSON body with an `error` message. The most common are `401` for an invalid API key, `400` for a missing required parameter such as `text`, and `429` when you exceed your hourly throughput limit or run out of searches. See [API status and error codes](https://serpapi.com/api-status-and-error-codes?utm_source=github_google_translate_scraper) for the full list.

## Output formats: JSON and Markdown

JSON is the default output and is useful when you need individual result fields. Add `output=md` to receive Markdown instead. It's more token-efficient, which makes it a good fit for LLMs and AI agents.

Markdown is plain text, not JSON, so read the response body as text instead of parsing it. Its structure doesn't follow the JSON field names.

| Client | Markdown request |
|--------|------------------|
| curl | `--data-urlencode output="md"` |
| Python | `client.search({..., "output": "md"})` |
| PHP | `$client->md([...])` |
| Rust | `client.md(parameter).await?` |

If your SDK doesn't have a Markdown helper yet, send a plain HTTP request with `output=md` and read the body as text. Learn more about [Markdown output](https://serpapi.com/markdown-output?utm_source=github_google_translate_scraper).

## Google Translate API parameters

See the [Google Translate API documentation](https://serpapi.com/google-translate-api?utm_source=github_google_translate_scraper) for more details and the [Google Translate languages](https://serpapi.com/google-translate-languages?utm_source=github_google_translate_scraper) page for supported language codes.

| Name | Description | Requirement |
|------|-------------|-------------|
| **Translation** | | |
| `text` | The text to translate, up to 5,000 characters. Emojis and some rare characters count as two. | Required |
| `from` | The language of the text: a two- or three-letter code, optionally followed by a region or script code (e.g., `es`, `pt-PT`, `zh-CN`). Use `auto` to detect the language. | Required |
| `to` | The language to translate into, using the same code format as `from`. | Required |
| **SerpApi parameters** | | |
| `engine` | Set to `google_translate`. | Required |
| `api_key` | Your private SerpApi API key. | Required |
| `no_cache` | Set to `true` to request fresh results instead of using the one-hour cache. Can't be combined with `async`. | Optional |
| `async` | Set to `true` for later retrieval through the [Searches Archive API](https://serpapi.com/search-archive-api?utm_source=github_google_translate_scraper). Can't be combined with `no_cache` or used with Ludicrous Speed enabled. | Optional |
| `zero_trace` | Enterprise only. Set to `true` to skip storing search parameters, files, and metadata on SerpApi servers. | Optional |
| `output` | Set to `json` (default), `html` for the raw HTML retrieved, or `md` for Markdown. | Optional |
| `json_restrictor` | Limit the JSON response to the fields you need. See the [JSON Restrictor](https://serpapi.com/json-restrictor?utm_source=github_google_translate_scraper) documentation. | Optional |

## Available data on Google Translate (JSON response)

Every response includes `translation.source` and `translation.target`. The field guides below describe the possible fields and their types; they aren't literal API responses.

### What you get depends on the text

Other fields appear only when Google Translate has data for them:

| Text | Additional fields |
|------|-------------------|
| More than one sentence | `translation.sentences` with source and target sentence pairs |
| A word or short phrase with dictionary data | `target.word_type`, `target.article`, `target.reverse_translations`, `more_translations`, `source.definitions`, `source.examples`, and `source.see_also` |
| A translation that depends on gender | `target.gender_specific_translations` with feminine and masculine variants |
| Misspelled text | `search_information.spelling_fix`, plus `search_information.showing_results_for` when the corrected text was translated |
| Text in a non-Latin script, or a single dictionary word | `source.pronunciation`, `target.pronunciation`, or both |
| A URL | `target.link` to the translated website on Google Translate |

With `from=auto`, the detected language is returned in `source.language` and `source.language_code`. If the text seems to be in a different language than the one set in `from`, Google Translate's suggestion is returned in `source.suggested_language`.

Each request translates a single text.

### Translation

```json
{
  "translation": {
    "source": {
      "language": "String - Source language name (detected when from=auto)",
      "language_code": "String - Source language code",
      "language_confidence": "Float - Detection confidence from 0 to 1",
      "text": "String - Text to translate",
      "pronunciation": "String - Phonetic spelling or romanization",
      "suggested_language": "String - Language Google suggests translating from",
      "suggested_language_code": "String - Code of the suggested language",
      "suggested_language_confidence": "Float - Confidence for the suggested language"
    },
    "target": {
      "language": "String - Target language name",
      "language_code": "String - Target language code",
      "text": "String - Translated text",
      "pronunciation": "String - Romanization of the translated text",
      "link": "String - Translated website on Google Translate, when the text is a URL"
    },
    "sentences": [
      {
        "source": "String - Sentence of the text to translate",
        "target": "String - Translated sentence"
      }
    ]
  }
}
```

### Dictionary data

```json
{
  "translation": {
    "source": {
      "definitions": [
        {
          "word_type": "String - Part of speech, e.g., adjective",
          "definition": "String - Definition of the text",
          "example": "String - Example of usage",
          "labels": "Array[String] - e.g., dated, humorous",
          "synonyms": [
            {
              "words": "Array[String] - Synonyms",
              "labels": "Array[String] - e.g., informal"
            }
          ]
        }
      ],
      "examples": "Array[String] - Example sentences with the text",
      "see_also": "Array[String] - Related words"
    },
    "target": {
      "word_type": "String - Part of speech of the translation",
      "article": "String - Article for nouns in languages that use them, e.g., le",
      "reverse_translations": "Array[String] - Source words that translate back to it",
      "gender_specific_translations": [
        {
          "text": "String - Translation for the gender",
          "gender": "String - feminine or masculine",
          "article": "String - Article of the translation",
          "reverse_translations": "Array[String] - Source words that translate back to it"
        }
      ]
    },
    "more_translations": [
      {
        "text": "String - Alternative translation",
        "article": "String - Article of the translation",
        "word_type": "String - Part of speech",
        "reverse_translations": "Array[String] - Source words that translate back to it"
      }
    ]
  }
}
```

When a translation is gender-specific, `article` and `reverse_translations` are stored in the matching `gender_specific_translations` item instead of `target`.

### Spelling corrections

```json
{
  "search_information": {
    "spelling_fix": "String - Spelling correction suggested by Google Translate",
    "showing_results_for": "String - Corrected text that was translated instead of the original"
  }
}
```

When `showing_results_for` is present, the `sentences` array contains the corrected text.

## AI agents and no-code

### MCP

Give AI agents and assistants a translation tool with the hosted [SerpApi MCP server](https://github.com/serpapi/serpapi-mcp) at `https://mcp.serpapi.com/mcp`.

Claude Code:

```bash
claude mcp add --transport http serpapi https://mcp.serpapi.com/mcp --header "Authorization: Bearer $SERPAPI_API_KEY"
```

Codex, which reads the key from `SERPAPI_API_KEY`:

```bash
codex mcp add serpapi --url https://mcp.serpapi.com/mcp --bearer-token-env-var SERPAPI_API_KEY
```

Other MCP clients:

```json
{
  "mcpServers": {
    "serpapi": {
      "type": "http",
      "url": "https://mcp.serpapi.com/YOUR_SERPAPI_API_KEY/mcp"
    }
  }
}
```

The server's engine catalog doesn't include Google Translate, but its `search` tool passes any engine through to SerpApi. Ask your agent to use the `google_translate` engine with `text`, `from`, and `to`. The resulting tool call looks like this:

```json
{
  "name": "search",
  "arguments": {
    "params": {
      "engine": "google_translate",
      "text": "I love coffee.",
      "from": "auto",
      "to": "es"
    }
  }
}
```

See the [MCP integration guide](https://serpapi.com/integrations/mcp?utm_source=github_google_translate_scraper) for more clients and self-hosting.

### CLI

Translate text from your terminal or from AI agent workflows with the [SerpApi CLI](https://github.com/serpapi/serpapi-cli):

```bash
brew tap serpapi/homebrew-tap
brew install serpapi-cli
```

The CLI picks up `SERPAPI_API_KEY` from your environment. You can also run `serpapi login` to save a key.

```bash
serpapi search engine=google_translate text="I love coffee." from=auto to=es
serpapi search engine=google_translate text="I love coffee." from=auto to=es --fields "translation.target.text"
serpapi search engine=google_translate text="I love coffee." from=auto to=es output=md
```

### No-code tools

Use SerpApi APIs without writing code:

- [NoCodeSerpApi](https://nocodeserpapi.com/)
- [SerpApi with n8n](https://serpapi.com/blog/tag/n8n/)
- [SerpApi with Make](https://serpapi.com/blog/tag/make/)
- [More integrations](https://github.com/serpapi/serpapi-integrations/)

## Use cases

- Translate product descriptions, reviews, and support content for international audiences.
- Detect the language of user-generated content before routing or analyzing it.
- Translate search results and reviews from other SerpApi APIs to research foreign markets.
- Give multilingual AI agents and RAG pipelines a translation tool.
- Build language-learning tools with definitions, examples, pronunciations, and gender-specific forms.
- Normalize misspelled queries with spelling corrections.

## Related scrapers

- [Google Scraper](https://github.com/serpapi/google-scraper): all Google scrapers from SerpApi
- [Google Maps Reviews Scraper](https://github.com/serpapi/google-maps-reviews-scraper): collect reviews to translate
- [Google Play Reviews Scraper](https://github.com/serpapi/google-play-reviews-scraper): collect app reviews to translate

## Contact

Feel free to reach out at [contact@serpapi.com](mailto:contact@serpapi.com).
