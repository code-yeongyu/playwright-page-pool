# Playwright Page Pool 🎭🏊‍♀️

[![PyPI version](https://badge.fury.io/py/playwright-page-pool.svg)](https://badge.fury.io/py/playwright-page-pool)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Welcome to Playwright Page Pool! 🎉

This Python module provides a convenient way to manage a pool of Playwright browser pages within a given browser context.
By leveraging Python's async capabilities, **you can handle multiple pages concurrently with ease and speed**, ensuring your tasks are executed swiftly and efficiently. 🚀
It allows you to efficiently reuse pages or create new pages on demand, making your web automation tasks a breeze! ✨

## Features 🌟

- **🌐 The best tool for working across multiple pages**
- 🏊‍♀️ Manages a pool of Playwright browser pages
- 🔄 Supports reuse of pages for improved performance
- 🆕 Creates new pages on demand when needed
- 🚀 Utilizes asynchronous operations for efficient execution
- 🔒 Ensures thread safety with semaphores and events
- 🎨 Customizable page initialization with `page_initiator` callback
- 🎯 Automatic closing of pages when the pool is no longer needed

## Installation 🔧

To start using Playwright Page Pool, simply install it via pip:

```sh
pip install playwright-page-pool
```

Make sure you have Playwright installed and set up in your project as well.

## Usage 📝

Here's a quick example of how to use Playwright Page Pool in your code:

```python
import asyncio

from playwright.async_api import async_playwright
from playwright_page_pool import PagePool

async def run_example(pool: PagePool) -> None:
    async with pool.acquire() as page:
        await page.goto("https://example.com")
        print(await page.title())

async def main(*args, **kwargs) -> None:
    async with async_playwright() as p:
        browser = await p.chromium.launch()
        context = await browser.new_context()

        async with PagePool(context=context, reuse_pages=True) as pool:
            run_examples = [run_example(pool) for _ in range(10)]
            await asyncio.gather(*run_examples)  # 10 tasks are executed concurrently

        await browser.close()


if __name__ == "__main__":
    asyncio.run(main())

```

In this example, we create a PagePool instance with a Playwright browser context. We set reuse_pages=True to enable page reuse for improved performance. We then define a run_example function that acquires a page from the pool using the acquire context manager, navigates to a URL, and prints the page title.
Finally, we create 10 tasks using run_example and execute them concurrently using asyncio.gather. The PagePool manages the allocation and reuse of pages efficiently.

## Configuration 🛠️

When creating a PagePool instance, you can customize its behavior with the following parameters:

- context: The Playwright browser context associated with the page pool.
- max_pages: The maximum number of pages that can be opened at once. Defaults to the number of CPU cores.
- page_initiator: An optional asynchronous callable that is called each time a new page is created. This allows you to perform custom initialization on each page.
- reuse_pages: Determines whether pages should be reused. If False, a new page is created for each acquisition. Defaults to False. When set to True, it eliminates the overhead of opening and closing pages repeatedly, thereby improving performance.

## Contributing 🤝

Contributions to Playwright Page Pool are welcome! If you find any issues or have suggestions for improvements, please open an issue or submit a pull request on the GitHub repository.

## License 📄

This project is licensed under the MIT License. See the LICENSE file for more details.
