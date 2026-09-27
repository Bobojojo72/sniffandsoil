# Design Toolkit: Install Check and How-To Guide

Checked 27 September 2026. Covers five tools: Taste Skill, Impeccable, Playwright CLI, Awesome Design MD and img2threejs.

---

## 1. Install check

Checked inside the Claude Code cloud session for the `sniffandsoil` repo. This is a fresh cloud machine, **not your Windows PC**, so it tells us nothing about what is on your laptop. Section 2 shows you how to check your own machine in about a minute.

| Tool | Status in cloud session | What was found |
|---|---|---|
| Taste Skill | Not installed | No `design-taste-frontend` (or any variant) in user or project skills |
| Impeccable | Not installed | No `impeccable` skill, plugin or `PRODUCT.md` |
| Playwright CLI | Partly | Playwright 1.56.1 library and Chromium are there. The agent tool `playwright-cli` (`@playwright/cli`) is **not** |
| Awesome Design MD | Not present | No `DESIGN.md` in the repo. This is a file library, not a program, so "installing" it means copying a file in |
| img2threejs | Not installed | No `img2threejs` folder in skills |

No Claude plugins are enabled on your claude.ai account either.

**Important:** anything installed in a cloud session disappears when the session ends. To have these tools in every cloud session, they need to live inside the repo (in `.claude/skills/`). To have them on your PC, install them there using Section 3.

---

## 2. Check your own PC (Windows 11)

1. Press **Windows key**, type `PowerShell`, press **Enter**.
2. Paste each line below, one at a time, and press **Enter** after each.

```powershell
dir $HOME\.claude\skills
playwright-cli --version
node --version
python --version
```

What you are looking for:

- In the skills list: `design-taste-frontend` (Taste Skill), `impeccable`, `img2threejs`, `playwright-cli`.
- `playwright-cli --version` prints a number. An error means it is not installed.
- `node --version` should be 18 or higher (needed for Playwright CLI, Impeccable and Taste Skill).
- `python --version` should be 3.10 or higher (needed for img2threejs).

Also check inside Claude Code: type `/plugin` and see if Impeccable is listed, and type `/` to see if `/impeccable` or `/img2threejs` appear.

If a project has its own skills, they live in that project's `.claude\skills` folder instead, so check there too.

---

## 3. Install and use each tool

### 3.1 Taste Skill

**What it is:** a skill that stops Claude producing generic "AI-looking" web pages (the Inter font, purple gradient, three-cards-in-a-row look). It pushes for distinctive typography, layout and motion.

**Install** (PowerShell, from inside your project folder):

```powershell
npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend"
```

When it asks which agent, choose **Claude Code**. It will also ask whether to install for this project only or globally: choose global if you want it in every project.

To get the whole family instead of just the main one, leave off the `--skill` part:

```powershell
npx skills add https://github.com/Leonxlnx/taste-skill
```

**The variants worth knowing:**

| Skill name | Use it when |
|---|---|
| `design-taste-frontend` | Default. Any new landing page, portfolio or site |
| `redesign-existing-projects` | Improving something that already exists (audits first, then fixes) |
| `high-end-visual-design` | You want calm, polished, "expensive" |
| `minimalist-ui` | Clean editorial look, Notion or Linear style |
| `industrial-brutalist-ui` | Bold, hard-edged, Swiss typography |
| `image-to-code` | You have a picture or mockup and want it built |
| `full-output-enforcement` | Claude keeps leaving "rest of code here" placeholders |

**The three dials:** at the top of the `design-taste-frontend` SKILL.md file are three settings from 1 to 10. Open the file in any text editor and change the numbers:

- `DESIGN_VARIANCE`: low = centred and tidy, high = asymmetric and experimental
- `MOTION_INTENSITY`: low = simple hover effects, high = scroll and magnetic animations
- `VISUAL_DENSITY`: low = spacious, high = packed dashboard

**How to use it:** skills trigger on their own when the task matches, so just ask normally. To force it, name it:

> Use the design-taste-frontend skill to redesign the Sniff & Soil homepage. Keep the eco, playful feel.

### 3.2 Impeccable

**What it is:** a design toolkit with 24 commands for building, critiquing and polishing front ends, plus a checker that scans for 61 common AI-design mistakes (overused fonts, purple gradients, poor contrast and so on).

**Install** (PowerShell, from your project folder):

```powershell
npx impeccable install
```

Then open Claude Code in that folder and run:

```
/impeccable init
```

This asks you about the product and writes a `PRODUCT.md` file, so every later command knows who the site is for.

Alternative install through Claude Code's plugin system: type `/plugin marketplace add pbakaus/impeccable`, then `/plugin` and install Impeccable from the list.

**The commands you will actually use:**

| Command | What it does |
|---|---|
| `/impeccable shape` | Plan the page before any code is written |
| `/impeccable craft` | Full plan-then-build with visual checks |
| `/impeccable critique` | Honest design review |
| `/impeccable audit` | Technical check: accessibility, speed, mobile |
| `/impeccable polish` | Final tidy before you publish |
| `/impeccable bolder` / `quieter` | Turn the design up or down |
| `/impeccable colorize` / `typeset` / `layout` | Fix just colour, just fonts or just layout |
| `/impeccable clarify` | Improve the wording on the page |
| `/impeccable document` | Write a `DESIGN.md` from your existing site |
| `/impeccable live` | Try design variations live in the browser |

**Shortcut:** `/impeccable pin audit` creates a plain `/audit` command so you type less.

**Recommended order:** `init`, then `shape`, then `craft`, then `audit`, then `polish`.

**Check a site without Claude at all:**

```powershell
npx impeccable detect https://www.sniffandsoil.co.uk
```

### 3.3 Playwright CLI

**What it is:** lets Claude drive a real browser from the command line: open pages, click, fill forms, take screenshots. It is the lighter, cheaper alternative to the Playwright MCP, because Claude only pulls in the page detail it needs.

Note: plain "Playwright" (the testing library) is a different thing. You want the agent tool `playwright-cli`.

**Install** (PowerShell):

```powershell
npm install -g @playwright/cli@latest
playwright-cli install-browser
```

Then, inside each project where you want Claude to use it:

```powershell
playwright-cli install --skills
```

That drops a skill into the project's `.claude\skills\playwright-cli` folder so Claude knows the commands.

**Main commands** (Claude runs these for you, but good to recognise):

| Command | What it does |
|---|---|
| `playwright-cli open https://example.com` | Opens a browser at that page |
| `playwright-cli goto <url>` | Goes to another page |
| `playwright-cli snapshot` | Reads the page and labels each button and field |
| `playwright-cli click <ref>` | Clicks a labelled element |
| `playwright-cli fill <ref> "text"` | Types into a field |
| `playwright-cli screenshot` | Saves a picture of the page |
| `playwright-cli -s=name <command>` | Uses a named session so logins are kept |

**How to use it:** just ask Claude, for example:

> Use playwright-cli to open the Sniff & Soil site, screenshot it at phone and desktop width, and tell me anything that looks broken.

### 3.4 Awesome Design MD

**What it is:** a free library of 70+ `DESIGN.md` files, each describing a well-known brand's design system (colours, fonts, spacing, feel) in plain text. Examples: Apple, Stripe, Notion, Linear, Spotify, Airbnb, Claude, Nike, Starbucks. Drop one into a project and Claude builds UI in that style.

Nothing to install. You copy one file.

**How to use it:**

1. Browse the library: https://github.com/VoltAgent/awesome-design-md (each brand is a folder under `design-md`).
2. In PowerShell, from your project folder, download the one you want. Example for Notion:

```powershell
curl.exe -o DESIGN.md https://raw.githubusercontent.com/VoltAgent/awesome-design-md/main/design-md/notion/DESIGN.md
```

Swap `notion` for the brand's folder name as it appears on GitHub.

3. Ask Claude:

> Read DESIGN.md and restyle the homepage to follow it, but keep our own logo, brand name and copy.

**Tip:** use it as a starting point, not a copy. Ask Claude to blend it with your own colours so the site does not look like someone else's brand. Impeccable's `/impeccable document` can then write a `DESIGN.md` that is truly yours.

### 3.5 img2threejs

**What it is:** give it a photo of an object and it rebuilds that object as a 3D model in code (Three.js) that runs in a web page and can be animated. No 3D files, just code.

**Needs:** Python 3.10 or higher and Git. Nothing else to install.

**Install** (PowerShell):

```powershell
git clone https://github.com/img2threejs/img2threejs.git "$HOME\.claude\skills\img2threejs"
```

Restart Claude Code afterwards so it picks the skill up.

**How to use it:**

1. Put the image in your project folder (on Windows, pasting images into the terminal is unreliable, so save the file and reference it with `@` instead).
2. In Claude Code:

```
/img2threejs @photos/dog-treat-jar.jpg Rebuild this object as a Three.js model, keep proportions, angles and colours.
```

3. Claude builds it in stages and compares its render against your photo at each stage, fixing differences as it goes.

**What you get back:** a TypeScript file that creates the model, a JSON description of its parts, and comparison images.

**Tips:**

- Say how accurate you need it up front ("rough is fine" or "must match closely").
- For animals, say what body type it is (for example "four-legged").
- Optional add-ons (character rigging, GLB export) install with `npx github:img2threejs/img2 install`.

---

## 4. How they fit together

A sensible flow for a website job such as a Sniff & Soil refresh:

1. **Pick a look:** grab a `DESIGN.md` from Awesome Design MD as inspiration.
2. **Set the brief:** `/impeccable init`, then `/impeccable shape`.
3. **Build:** ask Claude to build it. Taste Skill kicks in to avoid the generic look.
4. **Add a 3D moment (optional):** img2threejs turns a product photo into a spinning 3D model for the hero section.
5. **Check it for real:** Playwright CLI opens the site, screenshots phone and desktop, and clicks through the links.
6. **Finish:** `/impeccable audit`, then `/impeccable polish`.

---

## Sources

- Taste Skill: https://github.com/Leonxlnx/taste-skill
- Impeccable: https://github.com/pbakaus/impeccable and https://impeccable.style
- Playwright CLI: https://github.com/microsoft/playwright-cli
- Awesome Design MD: https://github.com/VoltAgent/awesome-design-md
- img2threejs: https://github.com/img2threejs/img2threejs
