# LLM Scraper Python

A Python port of the [LLM-powered web scraping library](https://github.com/mishushakov/llm-scraper), using `html2text` for HTML processing.

## Installation

```bash
pip install llmscraper
```

## Usage
```python
import asyncio
import instructor
from typing import List
from playwright.async_api import async_playwright
from pydantic import BaseModel
from llmscraper import LLMScraper

class Story(BaseModel):
    title: str
    url: str
    points: int
    by: str
    comments: int

class HackerNews(BaseModel):
    stories: List[Story]

async def main():
    client = instructor.from_provider("openai/gpt-4o-mini", async_client=True)
    scraper = LLMScraper(client)

    async with async_playwright() as p:
        browser = await p.chromium.launch()
        page = await browser.new_page()
        await page.goto("https://news.ycombinator.com")

        result = await scraper.run(
            page,
            schema=HackerNews,
        )
        print(result)

        await browser.close()

if __name__ == "__main__":
    asyncio.run(main())
```

## Examples

See the `examples/` directory for more usage examples.
