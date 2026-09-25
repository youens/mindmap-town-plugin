---
name: mind-maps
description: Draw a mind map in the chat with MindMap.Town. Use when someone asks for a mind map, wants to map something out, brainstorm or outline ideas visually, or turn notes, a plan, a checklist, or a document into a map. Also use to show, read, or change a mindmap.town link someone shares.
---

# Mind maps with MindMap.Town

MindMap.Town draws a mind map right in the conversation, in its candy style, with a button that opens the map in the free MindMap.Town editor at https://mindmap.town. The whole map lives in its link, and nothing is saved on a server.

## Draw a map

Call `show_mind_map`, the MindMap.Town connector's tool, with the whole map as `map`: a nested outline whose top topic is the central topic and whose `children` are the branches.

- Plan one central topic, 3 to 7 branches, and a few children under each. Group more than about 12 children under one topic into subtopics.
- Keep titles short (1 to 6 words), in the person's own words where they gave them. Put longer text in `notes`, which can use Markdown.
- These are defaults. Follow any size or shape the person asks for.
- For a checklist, give each item `todo` "open" or "done", and keep status words out of its title.
- Add a `link` only for an address you know is real.
- Leave `color` and `side` out unless the person asks, or to order a sequence (steps read down the right side, then down the left).

Once the map appears, the person can see it, so don't repeat it as text. Say in a sentence or two what it covers and offer a next step, such as tailoring it. Where the map can't be shown, such as in a terminal, give the link from the tool's result instead.

## Change a map

Call `show_mind_map` again with the complete updated map, not only the change. Keep every branch on the side it had: the previous result lists each branch with its side. A new map appears with the changes.

## Show a shared map

When someone shares a link that starts with `https://mindmap.town/#m=3&`, pass it to `show_mind_map` as `link` instead of `map`. The map appears, and you can then describe it or change it.

## When the tool isn't there

If `show_mind_map` isn't available, the MindMap.Town connector isn't connected. Write the map as a link yourself, following `references/link-guide.md`, and reply with that link, which opens the map in the browser. Mention that connecting MindMap.Town, from this plugin's Connectors tab, shows maps right in the chat.
