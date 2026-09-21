# Robots.txt Validator & Tester

**A free, browser-based tool to test whether a URL is allowed or blocked by robots.txt. Fetch a live file or paste your own, test it against 77 crawlers, bulk-check up to 100 paths, and see the exact rule and line that decides each result.**

[**Open the live tool**](https://makashibk.github.io/robots-txt-validator/)

![Live demo](https://img.shields.io/badge/demo-live-1a73e8?logo=googlechrome&logoColor=white)
![User agents](https://img.shields.io/badge/user%20agents-77-brightgreen)
![Dependencies](https://img.shields.io/badge/dependencies-none-brightgreen)
![License](https://img.shields.io/badge/license-MIT-green)

---

## Overview

Robots.txt Validator & Tester answers one question quickly: **"Can this crawler fetch this URL?"**

Enter a page URL, choose a crawler, and the tool loads the site's live `robots.txt` (or your own pasted content), applies the rules the way Google documents them, and shows a clear **Allowed** or **Blocked** result. The deciding rule is highlighted in the editor with its line number. You can also paste a whole list of URLs and check them all against the same file in one go.

It is a single HTML file with no framework, no build step, and no install.

## Features

### Two ways to load a robots.txt
- **Live mode (default)** downloads `https://your-domain/robots.txt` for the URL you enter. Fetches are cached per site for **45 seconds**, and the tool tells you when a result came from the cache.
- **Editor mode** lets you paste or type any robots.txt content and test it before you deploy. This works for staging sites, unpublished changes, and sites that can't be fetched.

### 77 user agents, plus your own
Pick from a searchable list, or search by name or token. If the crawler you need isn't listed, type its token and choose **Use custom** to test it anyway.

| Category | Count | Examples |
|---|---:|---|
| Search Engine Crawlers | 28 | Googlebot (plus Smartphone, News, Images, Video), Google-Extended, Google-InspectionTool, AdsBot-Google, Bingbot, Yahoo! Slurp, DuckDuckBot, Baiduspider, YandexBot, Applebot, Applebot-Extended |
| AI & LLM Crawlers | 17 | GPTBot, OAI-SearchBot, ChatGPT-User, ClaudeBot, Claude-Web, anthropic-ai, PerplexityBot, CCBot, cohere-ai, Meta-ExternalAgent, Bytespider, Amazonbot, DeepSeekBot |
| SEO & Analytics Bots | 13 | AhrefsBot, SEMrushBot, MJ12bot, DotBot, rogerbot, Screaming Frog, Sitebulb, Chrome-Lighthouse, DataForSeoBot |
| Social Media Bots | 10 | facebookexternalhit, Twitterbot, LinkedInBot, Pinterestbot, WhatsApp, TelegramBot, Discordbot, Slackbot |
| Monitoring & Misc | 9 | UptimeRobot, Pingdom, StatusCake, Site24x7, WPScan, Grammarly, Scrapy, W3C_Validator |

### Bulk URL check
Paste up to **100** URLs or paths, one per line, and check them all against the loaded robots.txt for the selected crawler. Each row shows an allowed or blocked mark, the path, and the matched rule with its line number (for example `disallow: /cart/ (L4)`). A summary at the top counts how many are allowed, how many are blocked, and the total.

You can enter full URLs (`https://example.com/blog/post`), paths (`/blog/post`), or paths without a leading slash (`blog/post`).

### Demo templates
Load a ready-made robots.txt into the editor to see common patterns in action:

| Template | What it shows |
|---|---|
| **Basic** | Simple allow and block rules, plus a `Sitemap` line |
| **E-commerce** | Blocks cart, checkout, account, internal search, and API paths, allows product and category pages, and blocks AhrefsBot |
| **Blog / CMS** | WordPress-style rules with an `admin-ajax.php` exception, and GPTBot and CCBot blocked |
| **Block AI Bots** | Denies GPTBot, ChatGPT-User, ClaudeBot, anthropic-ai, CCBot, PerplexityBot, Bytespider, and Amazonbot while leaving search engines allowed |

### Clear results
- A green **Allowed** or red **Blocked** result card names the path, the selected crawler and its token, and the rule and line number that decided it. If no rule matched, it says the URL is allowed by default.
- The editor shows **line numbers and syntax coloring** (directives, values, comments), jumps to the deciding line, and highlights it green or red.
- A **status pill** shows what happened: `Idle`, `Fetching`, `200 OK`, `404`, `Error`, `Loaded`, or `Editor`. The source URL is shown above the editor.

### Friendly input handling
- The URL box accepts `example.com/page` without a protocol and adds `https://` for you.
- It rejects a bare path such as `/page` (a full URL with domain is needed) and hostnames that aren't valid domains, with a clear message.
- **Enter** runs the test. In the crawler search, **Enter** picks the first match (or the custom entry), and **Esc** closes the list.

### Everything else
- **Dark mode** toggle. It follows your system setting the first time and remembers your choice afterward.
- A landing page with a **Start testing** button, and a **Back to home** button inside the tool.
- **Responsive** layout that adapts for tablets and phones.
- Link to Google's official robots.txt documentation.

## How to use

1. Open the [live tool](https://makashibk.github.io/robots-txt-validator/) and click **Start testing**.
2. Enter the URL you want to check, for example `https://example.com/blog/post-title`.
3. Choose a crawler from the **User agent / Bot** list. Googlebot is selected by default.
4. Click **Test URL**:
   - In **Live** mode, the tool fetches the site's robots.txt and shows the result.
   - In **Editor** mode, it tests the content you pasted.
5. Read the result card and check the highlighted line in the editor.
6. To test many URLs, open the **Bulk Check** tab, paste your list, and click **Check All**.

> **Tip:** In Live mode, run one single-URL test first. Bulk Check uses the robots.txt currently shown in the editor, so it needs that file to be loaded.

## How matching works

The tester follows the behavior Google documents for robots.txt:

| Behavior | How it's handled |
|---|---|
| Groups | Consecutive `User-agent` lines share the rules below them. Blank lines and comment-only lines are ignored |
| Comments | Text after `#` is ignored, including at the end of a line |
| Crawler selection | The group whose user-agent is the longest match for the selected crawler is used. Otherwise the `User-agent: *` group applies. A specific crawler such as `Googlebot-Image` falls back to a `Googlebot` group if it has no group of its own |
| `Allow` / `Disallow` | Both are supported |
| Wildcards | `*` matches any sequence of characters |
| End anchor | `$` at the end of a rule anchors it to the end of the URL |
| Most specific wins | The rule with the longest path pattern decides |
| Ties | If an `Allow` and a `Disallow` are equally specific, **`Allow` wins** |
| Empty `Disallow:` | Ignored, so everything is allowed |
| Case | Paths are matched case-sensitively |
| Query strings | The path is tested together with its query string |
| No matching rule | The URL is allowed |
| No robots.txt (404) | Everything is allowed, and the tool tells you so |

## About Live mode

The tool tries these sources in order and stops at the first one that works. Each attempt times out after 12 seconds:

1. **Direct request** from your browser to the site's `/robots.txt`. This only works if the site allows cross-origin requests.
2. **allorigins.win** (raw response)
3. **allorigins.win** (JSON response, which also reports the real HTTP status)
4. **corsproxy.io**

If a source returns an HTML page or a 404, the tool reports "robots.txt not found (404). All URLs allowed by default." If every source fails, it shows a network error and suggests copying the file into **Editor** mode.

## Privacy

- **Editor mode** runs entirely in your browser. Nothing you paste is sent anywhere.
- **Live mode** first requests the robots.txt directly from the site you're testing. If that's blocked, it falls back to the third-party proxy services listed above, which can see the address being fetched. Don't use Live mode for anything you need to keep private. Use Editor mode instead.
- The tool has no analytics, accounts, or cookies. Your dark or light theme choice is saved in your browser's local storage.
- The page loads *Inter* and *JetBrains Mono* from Google Fonts.

## Known limitations

- **It's a tester, not a linter.** It tells you what a crawler is allowed to do, but it doesn't flag typos, unknown directives, or malformed lines. `Sitemap:` and `Crawl-delay:` lines are displayed but not interpreted.
- **Groups for the same crawler aren't merged.** If a crawler appears in two separate groups, only the first one is used. Google combines them.
- **Broad crawler names can match a more specific group.** Selecting `Googlebot` can wrongly pick up a group written only for `Googlebot-Image` or `Googlebot-News`. Double-check results for files that mix crawler-specific groups.
- **Live mode depends on public proxies.** They can be slow, rate-limited, or blocked by some sites, and they may not receive the same response a real crawler would. Use Editor mode when accuracy matters.
- **Bulk Check handles up to 100 lines.** Anything beyond that is ignored.
- **robots.txt controls crawling, not indexing.** An allowed URL isn't guaranteed to be indexed, and a blocked URL can still appear in search results if other pages link to it. To keep a page out of the index, use `noindex` on a page crawlers can reach.
- **Confirm important decisions** with Google Search Console.

## Run locally

No install is needed. Open `index.html` in your browser, or serve the folder:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploy to GitHub Pages

1. Make sure the tool's file is named **`index.html`** and sits in the root of the `main` branch.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, select **`main`** and **`/ (root)`**, and click **Save**.
4. After a minute or two, the tool is live at `https://YOUR-USERNAME.github.io/robots-txt-validator/`.

## Project structure

```
.
├── index.html    # the whole tool: markup, styles, and script
├── README.md
└── LICENSE
```

## Customizing

Everything is in `index.html`.

**Add a crawler.** User agents live in the `UA_DATA` object, grouped by category. Each entry is `[display name, user-agent token, operator]`:

```js
["GPTBot", "GPTBot", "OpenAI"],
```

Add it to an existing category, or create a new key for a new category. It shows up in the dropdown and in search automatically.

**Add or edit a demo template.** Templates live in the `DEMOS` object as a `title`, a `desc`, and the robots.txt `content`. To show a new one, add a matching button in the **Demo Templates** tab that calls `loadDemo('yourKey')`.

## Contributing

Bug reports, missing crawlers, and pull requests are welcome.

1. [Open an issue](../../issues) describing the problem. For a wrong result, please include the robots.txt content, the URL you tested, and the crawler you selected.
2. For code changes, fork the repo, create a branch, make your change, and open a pull request explaining what changed and why.

## Related tools

Part of a small set of SEO tools. See them all at [makashibk.github.io](https://makashibk.github.io/), including the [SEO Extension for Chrome](https://github.com/makashibk/chrome-seo-extension).

## License

Released under the [MIT License](LICENSE).
