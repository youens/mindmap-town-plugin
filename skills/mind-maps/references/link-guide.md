<!-- MindMap.Town's agent guide, also at https://mindmap.town/agents.md -->

# Mind Map Town: make a map by writing a link

Mind Map Town (https://mindmap.town) is a free mind map editor that runs in the browser. A whole map lives in its link, in the part after the `#`. To make a map, write that link and give it to the person. They open it and get a map they can edit and share.

There is no API, server, account, or upload. Browsers never send the part after `#` to any server, so Mind Map Town never sees the map. You need no network access. Opening or downloading the link can't check it either (the server only ever sees `https://mindmap.town/`), so check it yourself (see Checklist).

## Recipe

1. Plan an outline: one central topic, 3 to 7 branches, a few children under each (at most about 12 under one node; group more into subtopics). Keep titles short (1 to 6 words) and put longer text in notes. Use the person's own words where they gave them (a capital first letter is fine); when they give only a subject, name the central topic and come up with the ideas yourself. These are defaults: when the person asks for a size or shape, follow it, within the Limits.
2. Number the nodes 0, 1, 2, ... in outline order. Node 0 is the central topic.
3. Write one record per node: `n=<number>;p=<parent number>;t=<title>`. The central topic has `p=-`.
4. On each branch (a direct child of the central topic), add `s=r` (right) or `s=l` (left). Put the first half on the right, rounding up, and the rest on the left: 3 branches go 2 right and 1 left; 5 go 3 right and 2 left.
5. Encode every title, note, and link value: spaces become `+`, `A-Z a-z 0-9 -` stay as they are, and every other character, periods included, becomes `%XX` escapes (see Encoding).
6. Join: `https://mindmap.town/#m=3&` followed by the records joined with `&`.
7. Check it (see Checklist), then hand it back (see Handing it back).

Smallest map: `https://mindmap.town/#m=3&n=0;p=-;t=Hello`

### Example 1: a simple map

```text
Birthday party
  Guests (right): Classmates, Cousins
  Food (right): Pizza, Cake
  Games (left): Treasure hunt, Musical chairs
  Decorations (left): Balloons, Banner
```

Records, one per line here for reading (the real link joins them with `&`):

```text
m=3
n=0;p=-;t=Birthday+party
n=1;p=0;t=Guests;s=r
n=2;p=1;t=Classmates
n=3;p=1;t=Cousins
n=4;p=0;t=Food;s=r
n=5;p=4;t=Pizza
n=6;p=4;t=Cake
n=7;p=0;t=Games;s=l
n=8;p=7;t=Treasure+hunt
n=9;p=7;t=Musical+chairs
n=10;p=0;t=Decorations;s=l
n=11;p=10;t=Balloons
n=12;p=10;t=Banner
```

```text
https://mindmap.town/#m=3&n=0;p=-;t=Birthday+party&n=1;p=0;t=Guests;s=r&n=2;p=1;t=Classmates&n=3;p=1;t=Cousins&n=4;p=0;t=Food;s=r&n=5;p=4;t=Pizza&n=6;p=4;t=Cake&n=7;p=0;t=Games;s=l&n=8;p=7;t=Treasure+hunt&n=9;p=7;t=Musical+chairs&n=10;p=0;t=Decorations;s=l&n=11;p=10;t=Balloons&n=12;p=10;t=Banner
```

## Fields

Records are joined with `&`, fields with `;`, and each field is `key=value`. `n` must come first; the rest may come in any order. The app silently drops a record that doesn't start with `n=` and any field it doesn't know (a typo like `note=` loses the note).

| Key | Meaning | Values |
|---|---|---|
| `n` | Node number | 0, 1, 2, ... each used once |
| `p` | Parent's number | a node number, or `-` for the central topic |
| `t` | Title, plain text (not Markdown) | encoded text |
| `notes` | Note shown when the node is opened; Markdown | encoded text |
| `link` | Web link on the node | a full address starting with `https://`, `http://`, or `mailto:`, then encoded (so it begins `https%3A%2F%2F`); anything else (like `www.example.com`) is kept but not shown |
| `td` | To-do checkbox | `0` open, `1` done (never `true` or `false`) |
| `s` | Side; only on branches, ignored elsewhere | `r` (default) or `l` |
| `c` | Color of this node and everything under it | `0` Tangerine (orange), `1` Blue Razz (blue), `2` Sour Apple (green), `3` Grape (purple), `4` Lemon Drop (gold), `5` Strawberry (pink), `6` Spearmint (teal) |
| `h` | Custom color, used instead of `c` | 6 hex digits, no `#`: `h=453e27` |
| `f` | Folded: children hidden until clicked; ignored on the central topic | `1` |

- **Levels and counts:** the central topic is level 0, branches are level 1, their children level 2, and so on. "4 levels of nesting" means nodes down to level 4. A requested number of ideas or items counts only those: "20 ideas in 4 groups" is 4 group nodes with 20 nodes under them.
- **Order:** siblings appear in order of their `n`. Each side stacks its branches top to bottom in that order, the left side too, so a sequence (steps, a timeline) reads down the right side, then down the left.
- **Colors** are automatic when left out, and each branch already gets its own: branches take colors 0, 1, 2, ... in `n` order (either side), and children share their branch's color. So "color the groups" needs no `c`. The fourth branch's Grape looks much like the central topic's violet; that's normal, or give it `c=5` (with 5 branches or fewer) to set it apart. Colors repeat after 7 branches; to give every group its own, use `h` from branch 8 on: `c9303e` cherry, `4c5fd5` indigo, `8d5a3b` cocoa, `66748a` slate. Automatic colors go by position only, so if you set `c` on some branches, pick colors the others don't have, or set it on all. A `c` on the central topic colors only that node.
- **To-dos:** put `td` on each item that should have a checkbox; children don't inherit it. An item the person marks as done or to do is a to-do (`td=1` if done), and so are its siblings, the other children of its parent (`td=0` unless marked done). Drop status words like "(done)" from the titles. Make the list's header a plain node: with two or more to-do children it shows a progress pie, and it counts as done when they all are. Pies roll up, so when two or more branches hold to-do lists, the central topic shows their overall progress (branches without to-dos don't count).
- **Notes** are GitHub-style Markdown: paragraphs, `- ` lists, `**bold**`, `*italics*`, code, tables, `> ` quotes, and links. Newlines are `%0A`. Put a blank line (`%0A%0A`) between paragraphs; in the note's preview, one newline inside a paragraph shows as a space. Links must be `http`, `https`, or `mailto`. Images and HTML are not shown. Some plain text reads as Markdown: a line that starts with `# `, `- `, `+ `, `* `, `> `, or a number and a period (`1. `, `2024. `) becomes a heading, list, or quote, and `*`, `_`, or `~` around words changes their style. A backslash before such a character keeps it as typed (`\+`, encoded `%5C%2B`).
- **Links:** put a link on the node it's about (a museum's website goes on that museum's node). Add only addresses you know are real; nothing checks that a page exists. If you aren't sure of a deeper page, use the site's home page, or leave the link out and say so.
- **Rarely needed:** `x` and `y` place a central topic, `w` sets a node's width in pixels, `conn=<n>` draws a dotted line to another node, and the app adds `a=<0-6>` to pin each branch color. Leave them out of new maps; keep them as they are in existing ones.

## Encoding

Encode each `t`, `notes`, and `link` value as UTF-8 percent-encoding with `+` for spaces. Keep the ASCII letters `A-Z` and `a-z`, the digits `0-9`, and `-` as they are, and encode everything else, `.` `_` `~`, accented letters, and emoji included. Encode each value once, and never encode the `&`, `;`, and `=` between records and fields. Other values (numbers, `-`, `r`, `l`) never need encoding.

| Character | Write | If left raw |
|---|---|---|
| space | `+`, always | the map opens, but chat apps end the link at the first space |
| `&` | `%26` | the text is cut off there |
| `;` | `%3B` | the text is cut off there |
| `+` | `%2B` | turns into a space: in a finished link, every `+` is a space |
| `%` | `%25` | it shows as typed, unless it starts a valid escape: `50%20off` shows `50 off` |
| `#` | `%23` | some apps cut the link there |
| `=` `,` `:` `/` `?` `@` `>` | `%3D` `%2C` `%3A` `%2F` `%3F` `%40` `%3E` | fine, but encode anyway |
| `.` `_` `~` | `%2E` `%5F` `%7E` | a link that ends in `.` gets cut in iMessage, often right after the first node or two |
| `'` `(` `)` `*` `!` `"` | `%27` `%28` `%29` `%2A` `%21` `%22` | a lone `(` or `)`, or a link ending in `!` or `)`, gets cut in iMessage and Markdown |
| newline | `%0A` | |
| accented letters, emoji, other scripts | every UTF-8 byte: e-acute is `%C3%A9`, the party popper emoji is `%F0%9F%8E%89` | browsers cope, but some apps break the link there |

Many emoji are more than one character: a flag is two, and many emoji end with an invisible variation selector (U+FE0F, `%EF%B8%8F`). Encode every one of them.

In Python: `quote_plus(text, safe="").replace(".", "%2E").replace("_", "%5F").replace("~", "%7E")`, with `quote_plus` from `urllib.parse` (alone, it keeps `. _ ~`). In JavaScript: `encodeURIComponent(text).replace(/[!'()*._~]/g, (c) => "%" + c.charCodeAt(0).toString(16).toUpperCase()).replace(/%20/g, "+")`.

## More examples

### Example 2: notes, a link, and special characters

Central topic `Launch: Q4 & beyond` with a Markdown note (a `## Goal` heading, a bold word, a `100%`, a two-item list, and a `> ` quote). Branch `Research` links to `https://example.com/search?q=mind+maps&lang=en` and has children `Interviews; 5 done` and `Survey (draft)`. `Build` has `API + docs` and `C# client`. `Tell people` (left) has a note and children `Blog post` and `Email: "It's here!"`.

```text
https://mindmap.town/#m=3&n=0;p=-;t=Launch%3A+Q4+%26+beyond;notes=%23%23+Goal%0AShip+%2A%2Av2%2A%2A+to+100%25+of+users%2E%0A%0A-+Owner%3A+Sam%0A-+Budget%3A+%245k%0A%0A%3E+%22Small+steps%2C+every+week%2E%22&n=1;p=0;t=Research;link=https%3A%2F%2Fexample%2Ecom%2Fsearch%3Fq%3Dmind%2Bmaps%26lang%3Den;s=r&n=2;p=1;t=Interviews%3B+5+done&n=3;p=1;t=Survey+%28draft%29&n=4;p=0;t=Build;s=r&n=5;p=4;t=API+%2B+docs&n=6;p=4;t=C%23+client&n=7;p=0;t=Tell+people;notes=Post+on+the+blog%2C+then+email%2E;s=l&n=8;p=7;t=Blog+post&n=9;p=7;t=Email%3A+%22It%27s+here%21%22
```

### Example 3: to-dos, a color, and a folded branch

`Checklist` is a plain header with three to-do children (one done), so it shows a progress pie. `Neighborhoods` is Strawberry pink (`c=5`, a color no other branch has), and `Paperwork` starts folded.

```text
https://mindmap.town/#m=3&n=0;p=-;t=Move+to+Lisbon&n=1;p=0;t=Checklist;s=r&n=2;p=1;t=Book+movers;td=1&n=3;p=1;t=Transfer+utilities;td=0&n=4;p=1;t=Update+address;td=0&n=5;p=0;t=Neighborhoods;s=r;c=5&n=6;p=5;t=Alfama&n=7;p=5;t=Estrela&n=8;p=5;t=Campo+de+Ourique&n=9;p=0;t=Paperwork;s=l;f=1&n=10;p=9;t=Visa&n=11;p=9;t=Tax+number+%28NIF%29&n=12;p=9;t=Bank+account
```

## If you can run code

Save the code below as `mindmap_town.py` (only the code, not the ``` lines around it), then `from mindmap_town import *`. `link = nodes_to_link(outline_to_nodes(outline))` builds a link from a nested outline. `link_to_nodes(link)` reads a link the way the app does: it prints a summary (nodes, levels, to-dos, folded branches, length) and the outline, with notes and each branch's color, and raises `ValueError` for anything that would break the map or damage its text. Run it on every link you hand back, and reread what it prints.

```python
import re
from urllib.parse import quote_plus, unquote_to_bytes

COLORS = ["Tangerine", "Blue Razz", "Sour Apple", "Grape", "Lemon Drop", "Strawberry", "Spearmint"]

def outline_to_nodes(root):
    """root = {"title": str, "notes": str, "link": "https://...", "todo": False (open)
    or True (done), "color": 0 to 6, "side": "r" or "l", "folded": True, "children": [...]}.
    Only "title" is required. Branch sides default to the first half (rounded up) on the right."""
    nodes = {}
    def add(node, parent, level, side):
        n = len(nodes)
        x = nodes[n] = {"p": "-" if parent is None else str(parent), "t": node["title"]}
        if node.get("notes"): x["notes"] = node["notes"]
        if node.get("link"): x["link"] = node["link"]
        if node.get("todo") is not None: x["td"] = "1" if node["todo"] else "0"
        if level == 1: x["s"] = node.get("side", side)
        if node.get("color") is not None: x["c"] = str(node["color"])
        if node.get("folded") and level > 0: x["f"] = "1"
        kids = node.get("children", [])
        for i, kid in enumerate(kids):
            add(kid, n, level + 1, "r" if i < (len(kids) + 1) // 2 else "l")
    add(root, None, 0, "r")
    return nodes

def encode(text):
    """Encodes a t, notes, or link value: only A-Z a-z 0-9 - stay as they are."""
    return quote_plus(text, safe="").replace(".", "%2E").replace("_", "%5F").replace("~", "%7E")

def nodes_to_link(nodes):
    """{number: {field: value}} -> link. Encodes t, notes, and link; writes other values as they are."""
    enc = lambda k, v: encode(v) if k in ("t", "notes", "link") else v
    return "https://mindmap.town/#m=3&" + "&".join(
        ";".join([f"n={n}"] + [f"{k}={enc(k, v)}" for k, v in x.items()]) for n, x in sorted(nodes.items()))

def link_to_nodes(link, given=False):
    """Link -> {number: {field: value}}, text decoded. Prints a summary and the outline;
    raises ValueError. given=True allows the extra central topics a map you were given may have."""
    head, _, frag = link.strip().partition("#")
    if head not in ("https://mindmap.town/", "https://mindmap.town/map") or not re.match(r"m=[13]&", frag):
        raise ValueError("the link must start with https://mindmap.town/#m=3&")
    if re.search(r"[\s#]", frag): raise ValueError("raw space or # in the link")
    if not given and re.search(r"[._~!'()*]", frag): raise ValueError("raw . _ ~ ! ' ( ) or * in the text: chat apps cut the link")
    nodes, known = {}, {"p", "t", "notes", "link", "td", "s", "c", "h", "f", "x", "y", "w", "a", "conn"}
    for record in filter(None, frag.split("&")[1:]):
        first, *fields = record.split(";")
        if not re.fullmatch(r"n=(0|[1-9]\d*)", first): raise ValueError(f"not a node (raw & or ;?): {record[:60]}")
        n = int(first[2:])
        if n in nodes: raise ValueError(f"{first} is used twice")
        x = nodes[n] = {}
        for field in fields:
            key, eq, value = field.partition("=")
            if not eq or key not in known: raise ValueError(f"{first}: unknown field {field[:40]} (raw ;?)")
            if key in ("t", "notes", "link"):
                if re.search(r"%(?![0-9A-Fa-f]{2})", value): raise ValueError(f"{first}: raw % in {key}")
                value = unquote_to_bytes(value.replace("+", " ")).decode("utf-8")
            x[key] = value
    rules = {"p": r"-|0|[1-9]\d*", "c": r"[0-6]", "a": r"[0-6]", "td": r"[01]", "s": r"[rl]", "f": "1",
             "h": r"[0-9a-fA-F]{6}", "link": r"(https?://|mailto:)\S+"}
    for n, x in nodes.items():
        if "p" not in x: raise ValueError(f"n={n} has no p=")
        for key, rule in rules.items():
            if key in x and not re.fullmatch(rule, x[key]): raise ValueError(f"n={n}: bad {key}={x[key][:40]}")
        if x["p"] != "-" and int(x["p"]) not in nodes: raise ValueError(f"n={n}: p={x['p']} is not a node")
    if len(nodes) > 1000: raise ValueError("more than 1,000 nodes")
    if sum(x["p"] == "-" for x in nodes.values()) > 1 and not given:
        raise ValueError("more than one node has p=-")
    lines, seen = [], []
    def show(parent, level):
        kids = [n for n in sorted(nodes) if nodes[n]["p"] == parent]
        for i, n in enumerate(kids):
            if level > 12: raise ValueError(f"n={n} is more than 12 levels deep")
            x = nodes[n]
            seen.append(level)
            extra = {k: v for k, v in x.items() if k not in ("p", "t", "notes")}
            if level == 1:
                extra["color"] = "#" + x["h"] if "h" in x else COLORS[int(x.get("c", x.get("a", i % 7)))]
            lines.append("  " * level + (x.get("t") or "(no title)") + (f"  {extra}" if extra else ""))
            if x.get("notes"): lines.append("  " * level + "  | " + x["notes"].replace("\n", "\n" + "  " * level + "  | "))
            show(str(n), level + 1)
    show("-", 0)
    if len(seen) < len(nodes): raise ValueError("no node has p=-, or some parents form a loop")
    todos = [x["td"] for x in nodes.values() if "td" in x]
    print(f"OK: {len(nodes)} nodes, {max(seen)} levels below the central topic, {len(todos)} to-dos "
          f"({todos.count('1')} done), {sum('f' in x for x in nodes.values())} folded, {len(link.strip())} characters")
    print("\n".join(lines))
    return nodes
```

## Limits

| Limit | Maximum |
|---|---|
| Nodes | 1,000 |
| Depth | 12 levels below the central topic |
| One title / one note / one node link | 10,000 / 12,000 / 4,096 characters |
| All notes together | 240,000 characters |
| Everything after `#` | 1,500,000 characters |

Titles, notes, and links are measured after decoding, the way JavaScript counts: most characters count 1, and an emoji counts 2 or more. The last limit counts the encoded text.

Most maps read best with 10 to 80 nodes, but make a bigger one when asked. Chat apps and email can cut long links, so above about 8,000 characters, trim the notes or tell the person to copy the whole link. For a big or compact map, keep titles to a few words and leave out notes and optional fields. Folding (`f=1`) hides a branch's children until clicked, so fold only when asked.

## Checklist

A link that breaks a rule opens as a blank map instead of yours, with no explanation, and some mistakes silently drop text. If you can run code, `link_to_nodes` checks all of this. Otherwise, before replying, check:

- It starts with `https://mindmap.town/#m=3&` (a `#`, never a `?`).
- Every record starts with `n=`. Numbers are unique, every `p` is `-` or an existing node's number, exactly one node has `p=-` (unless a map you were given already had more), and following parents never loops.
- Every title, note, and link is encoded: no raw space, `&`, `;`, `%`, `#`, `.`, or parenthesis in any of them, and every `%` begins a valid `%XX` escape. Every `+` stands for a space, so a real plus sign must be `%2B`. In particular, the link must not end in `.`.
- Only the fields above, with allowed values: `td` is 0 or 1, `s` is r or l, `c` is 0 to 6, `h` is six hex digits, and each `link`, decoded, starts with `https://`, `http://`, or `mailto:`.
- It's inside the limits.
- No passwords, keys, or other secrets unless the person asked for them. Anyone with the link can read the map.

When the map opens, the app tidies the link (renumbers nodes and adds fields like `a=`). That's normal.

## Changing a map someone gives you

With code: `nodes = link_to_nodes(link, given=True)` (`given=True` allows the extra central topics some maps have), change `nodes` (values are strings, like `nodes[13] = {"p": "0", "t": "Music", "s": "r"}`), check `link_to_nodes(nodes_to_link(nodes), given=True)`, and hand back `nodes_to_link(nodes)`. By hand:

1. Take the part after `#`. Split it on `&` into records, and each record on `;` into fields. Split each field at its first `=`. (Older links start with `m=1`; read them the same way and write `m=3`.)
2. Decode `t`, `notes`, and `link`: turn `+` into a space, then percent-decode. Leave other values as they are.
3. Change, add, or remove records. Give new nodes unused numbers. A node's number sets its place among its siblings, so the highest number puts it last, and a parent may have a higher number than its children. Keep every field you aren't changing exactly as it was, including `a`, `x`, `y`, `w`, and `conn`. When removing a node, remove its children too or give them a new parent.
4. Re-encode, rejoin, and hand back the whole new link. The old link still opens the old version.

For example, adding `&n=13;p=0;t=Music;s=r` to the end of Example 1 adds a `Music` branch on the right.

## Handing it back

- Make the link clickable. Where Markdown renders: `[Open the map: Birthday party](https://mindmap.town/#m=3&...)`. In a terminal or plain text: the bare link on its own line, with no punctuation touching its end.
- Never run the map through a link shortener, paste site, or file host. They would store the map on their servers.
- Don't say the map was saved or uploaded. Nothing was; the link is the map.
- Add a sentence or two, like: "It opens in your browser, ready to edit. The whole map lives in this link, so bookmark it, and after you edit, copy the new link from the address bar or the Share button. Anyone with the link can read the map, so keep passwords and secrets out."
- Mention what the person might not spot: which color each group got, if they asked for colors; which branches start folded; and any link you left out or weren't sure of.

Privacy: Mind Map Town never receives maps or anything the person tells you. What they share with you is handled by you and your provider, under your provider's terms.
