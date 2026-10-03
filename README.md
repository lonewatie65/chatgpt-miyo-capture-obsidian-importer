# Selectively Import ChatGPT Conversations into Obsidian with Miyo + macOS Shortcuts

This workflow uses **Miyo Capture** to export ChatGPT conversations as Markdown, then a simple **macOS Shortcut** to choose individual conversations from the export and move them into any folder you want in your Obsidian vault.

> This is an independent community workflow and is not affiliated with or endorsed by Miyo/Brevilabs, OpenAI, or Obsidian.

## Install the Shortcut

If you don't want to build it manually, you can install the ready-made Shortcut:

**[Install Miyo Capture ChatGPT Obsidian Importer macOS Shortcut](https://www.icloud.com/shortcuts/8bbef99bbe5b4a2c997aa50d26d3ce3b)**

During installation, Shortcuts asks you to choose the Obsidian folder where imported ChatGPT conversations should be saved. The shared Shortcut does not contain a hard-coded personal vault path.

---

## Requirements

- macOS
- Apple Shortcuts
- Obsidian (if using this specifically for an Obsidian vault)
- Google Chrome
- Miyo Capture Extension for Chrome

> The Miyo Capture Extension currently depends on Chrome for capturing/exporting ChatGPT conversations.

## What Miyo Provides

The Miyo Capture Extension exports ChatGPT conversations as individual Markdown files.

The exported Markdown includes useful frontmatter such as:

- `platform`
- `conversation_id`
- `title`
- `url`
- `created_at`
- `updated_at`

Conversation filenames also include the date and title, which makes them easy to identify when selecting them in the Shortcut.

Example:

`2026-09-27 Network storage setup (6ab88ace).md`

The Miyo Capture Extension can export conversations using a date range, including a custom date range.

The limitation is that the custom export selection in the extension is date-based rather than letting you select individual conversation titles.

The Shortcut below adds that missing selection step.

---

# Build the macOS Shortcut

## 1. Create a new Shortcut

Open **Apple Shortcuts** on macOS and create a new Shortcut.

Give it any name you like.

Example:

`Import ChatGPT Conversations`

---

## 2. Add "Select Files"

Add:

**Select Files**

This lets you manually choose the Miyo ZIP file when the Shortcut runs.

The ZIP can live wherever you prefer, for example:

- Downloads
- Desktop
- Documents
- an archive folder

There is no need to hard-code the ZIP location.

---

## 3. Add "Extract File"

Immediately after **Select Files**, add:

**Extract File**

The input should be the file selected in the previous action.

The Shortcut now looks like:

1. Select Files
2. Extract File

macOS handles the extraction for the workflow. You do not need to manually unzip the Miyo export first.

---

## 4. Add "Choose from List"

Add:

**Choose from List**

Its input should be the **Files** output from the Extract File action.

Expand the action and configure:

- Prompt: `Choose conversations to import`
- **Select Multiple: ON**
- Select All Initially: optional

When the Shortcut runs, this produces a list containing the conversation filenames exported by Miyo.

Because the Miyo Capture Extension includes the conversation date and title in the filename, the list becomes a convenient conversation picker.

Example:

- `2026-09-25 Obsidian Rebuild Continue (...)`
- `2026-09-27 Painter Genealogy Research (...)`
- `2026-09-27 Network storage setup (...)`

Select only the conversations you actually want to import.

---

## 5. Add "Repeat with Each"

Add:

**Repeat with Each**

Set its input to the output from **Choose from List**.

The Shortcut should now show something similar to:

`Repeat with each item in Selected Item`

This is important when multiple conversations are selected.

---

## 6. Add "Move File" INSIDE the Repeat block

Inside:

`Repeat with each item in Selected Item`

add:

**Move File**

For the file being moved, use the Shortcuts magic variable:

**Repeat Item**

Do NOT use `Selected Item` here.

It should read:

`Move Repeat Item to [destination]`

---

## 7. Choose your destination folder

For the destination in **Move File**, select whatever folder you want.

For an Obsidian workflow, this can be any folder inside your vault.

Example:

`ChatGPT Conversations`

The destination is entirely user-selectable. It does not need to have this name or be located at the root of the vault.

---

## 8. Optional: Enable "Replace Existing Files"

Expand the **Move File** action.

Enable:

**Replace Existing Files**

This is useful if you intend to export the same ChatGPT conversation again later.

Because the Miyo Capture Extension includes the conversation identifier in the filename, a newer export of the same conversation can replace the older Markdown copy rather than creating another copy.

This makes it possible to refresh archived conversations as they continue to grow.

Leave this disabled if you would rather preserve existing files.

---

## 9. Add "End Repeat"

The Repeat block should now be:

1. Repeat with each item in Selected Item
2. Move Repeat Item to your chosen destination
3. End Repeat

---

## 10. Add "Stop This Shortcut"

After **End Repeat**, add:

**Stop This Shortcut**

This is important.

Without it, Shortcuts may attempt to display the output generated by the file operations. Large ChatGPT Markdown conversations can result in a very large result preview and may cause Shortcuts to become sluggish or show a spinning beach ball.

Adding **Stop This Shortcut** gives the workflow a clean endpoint.

---

# Finished Shortcut

The complete Shortcut is:

1. **Select Files**
2. **Extract File**
3. **Choose from List**
   - Prompt: `Choose conversations to import`
   - Select Multiple: ON
4. **Repeat with each item in Selected Item**
   - **Move Repeat Item to [your destination folder]**
   - Optional: Replace Existing Files ON
5. **End Repeat**
6. **Stop This Shortcut**

---

# Using It

1. Open ChatGPT in Chrome.
2. Install the Miyo Capture Extension
3. Sign into your ChatGPT account as needed
4. Use the Miyo Capture Extension to export the desired date range.
5. Save/download the resulting ZIP wherever you prefer (there's no need to extract the zip file)
6. Run the macOS Shortcut.
7. Select the Miyo ZIP.
8. The Shortcut extracts the archive.
9. A list of conversation dates/titles appears.
10. Select the conversations you want.
11. Click **Done**.
12. Only those selected Markdown files are moved into your chosen destination folder.

The original ZIP can remain wherever you downloaded it.

If the destination is inside an Obsidian vault, the conversations immediately become ordinary searchable Markdown notes.

---

## Why bother?

The Miyo Capture Extension already does the difficult part very well: converting ChatGPT conversations into clean Markdown with useful metadata.

The Shortcut adds one useful layer:

**selective importing by conversation title.**

Instead of importing every conversation contained in a date-range export, you can use the Miyo Capture Extension to capture a broad period and then choose exactly which conversations deserve a place in your vault.

Once imported into Obsidian, old ChatGPT conversations can be searched alongside the rest of your notes, making it much easier to rediscover previous research, troubleshooting, projects, and discussions.
