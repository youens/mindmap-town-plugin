# MindMap.Town for Claude

![MindMap.Town](assets/icon.png)

Turn any idea into a colorful mind map you can open and edit. Ask Claude for a mind map and MindMap.Town draws it right in the chat: branches, notes, links, and to-do checklists, in a cozy candy style. One click opens it in the free [MindMap.Town](https://mindmap.town) editor, where you can rearrange it, recolor it, check things off, and share it.

## What's inside

- **The MindMap.Town connector** at `https://mindmap.town/mcp`, with one read-only tool, `show_mind_map`, which draws the map in the conversation. It needs no account or sign-in.
- **The mind-maps skill**, which tells Claude when to draw a map, how to shape it, and how to change it. When the connector isn't connected, it has Claude write the map as a MindMap.Town link instead, following MindMap.Town's [agent guide](https://mindmap.town/agents.md).

## Use it

1. Add the plugin, then connect MindMap.Town from the plugin's **Connectors** tab. There's nothing to sign in to.
2. Ask for a map. For example:
   - Mind map the pros and cons of moving to a new city
   - Turn my meeting notes into a mind map
   - Map out my move as a checklist
   - Brainstorm birthday party ideas as a mind map
3. Open notes on the map, go full screen to zoom and pan, or select **Open in Mind Map Town** to edit it.
4. To change the map, just ask. Claude sends the whole updated map again, and a new one appears.

Shared a map link? Paste a `https://mindmap.town/#m=3&...` link and ask Claude to show it.

## What it sends

When Claude draws a map, it sends the map it wrote (titles, notes, links, and to-dos, which can include things from your conversation) to the MindMap.Town connector at `https://mindmap.town/mcp`. The connector checks the map, answers with its link, and keeps nothing: no storage, no logs, and no accounts. It never receives your conversation itself, only the map.

The map viewer runs inside Claude, loads only MindMap.Town's fonts from `https://mindmap.town`, and connects nowhere else. The map lives in its link, in the part after the `#`, which browsers never send to a server, so opening it in the editor sends nothing to MindMap.Town either.

Without the connector, the skill has Claude write the link right in the chat, and nothing is sent anywhere.

Read more in the [Privacy Policy](https://mindmap.town/privacy).

## Help

Email [hello@mindmap.town](mailto:hello@mindmap.town), or see [mindmap.town/agent](https://mindmap.town/agent).

## License

[MIT](LICENSE)
