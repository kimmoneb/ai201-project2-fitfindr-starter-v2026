# FitFindr

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, every
> command, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> python app.py listings --full -n 6      # read the data (Milestone 1)
> python app.py fields                    # what you can filter on
> python app.py ask 'vintage graphic tee under $30'
> ```
>
> All three tools are stubs, so that last command will do nothing useful yet.
> That's the starting position.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     This is your submission. Fill each section in as you finish the milestone
     it belongs to — don't leave it all to the end.

     Unit 3 asks for the first five sections. Unit 4 adds the five below them.
     Leave the unit 4 sections alone until then; they're here so you know
     what's coming.

     Everything is pasted as TEXT. No screenshots, no images, no video links.
     A typed block of output gets full credit; a picture of the same output
     gets none.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 3 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Three or four sentences: what a user asks for, and what they get back. -->



---

## Tool Inventory

<!-- Four lines per tool. This is worth 2 points and it's the single most
     common place students lose them.

     "Returns a list" earns NOTHING. The description has to say what is IN
     the list.

     The empty case isn't optional either — it's the thing your loop branches
     on, and if you don't decide it here you'll discover it as a crash in
     Milestone 5. -->

### `search_listings`

- **What it does:** Searches the listings data for items matching a description, with optional size and maximum price filters.
- **Inputs:** `description` (str), `size` (str or None), `max_price` (float or None)
- **Returns:** A `list[dict]` containing the matching clothing listings.
- **When it has nothing:** Returns an empty list'[]'.

### `suggest_outfit`

- **What it does:** Suggests one or two outfits using the thrifted item the user is considering and items from the user's wardrobe.
- **Inputs:** `new_item` (dict), `wardrobe` (dict)
- **Returns:** A non-empty string containing outfit suggestions.
- **When it has nothing:** If the wardrobe is empty, returns general styling advice for the new item instead of failing or returning an empty string.

### `create_fit_card`

- **What it does:** Creates a short two-to-four sentence caption about the thrifted item and the suggested outfit.
- **Inputs:** `outfit` (str), `new_item` (dict)
- **Returns:** A string containing a two-to-four sentence caption.
- **When it has nothing:** If `outfit` is empty or whitespace, returns a descriptive message instead of raising an error.

---

## Planning Loop

<!-- Your branch rule, stated as a rule — the condition AND both paths — plus
     the file and function that holds it.


     Like this:
       "If search_listings returns an empty list, put a message in the session
        and stop. Otherwise take the first result and go to suggest_outfit."
        — agent.py::run_agent

     The grader checks your code against what you claim here, so the file and
     function have to be real. -->

**Branch rule:** If `search_listings` returns an empty list, store a message in the session and stop. Otherwise, select the first result, pass it to `suggest_outfit`, and then pass the outfit suggestion to `create_fit_card`.

**Where it lives:** `agent.py::run_agent`

**How the query is parsed:** The model parses the user's query into the description, size, and maximum price needed by `search_listings`.

**What moves through the session:** The session stores the search results first, then the selected item, then the outfit suggestion, and finally the fit-card caption.
---

## Sample Run


<!-- Two things go here.

     1. One FULL query and its output, pasted as text.
     2. Your three per-tool terminal tests — the command and what it printed. -->

**One full query**

```
$ python app.py ask 'vintage graphic tee under $30'
The agent found a matching vintage graphic tee, generated outfit suggestions, and created a fit-card caption with styling ideas.

```

**The three tools, tested one at a time**

```
1. search_listings

$ python -c "from tools import search_listings; print(search_listings('vintage graphic tee', 'M', 30))"

Output: Returned a list of matching listings for a vintage graphic tee in size M under $30.

2. suggest_outfit

$ python -c "from tools import search_listings, suggest_outfit; item=search_listings('vintage graphic tee','M',30)[0]; print(suggest_outfit(item, {}))"

Output: Returned two outfit suggestions with bottoms, footwear, accessories, and an explanation of why each outfit works, even with an empty wardrobe.

3. create_fit_card

$ python -c "from tools import search_listings, suggest_outfit, create_fit_card; item=search_listings('vintage graphic tee','M',30)[0]; outfit=suggest_outfit(item, {}); print(create_fit_card(outfit, item))"

Output: Returned a fit-card caption for the selected item with styling advice, the $18.00 price, and Depop platform.

```

```
$ python -c "from tools import suggest_outfit; ..."Here are two outfit combinations using the vintage Levi's 501s and pieces already in your wardrobe:

### Outfit 1: Effortless Casual Streetwear
*Pair the medium-wash denim with a cozy, relaxed top and chunky sneakers for an easy, everyday look.*
* **Top:** Oversized grey crewneck sweatshirt (`w_004`)
* **Bottoms:** Vintage Levi's 501 Jeans — Medium Wash (`lst_001`)
* **Shoes:** Chunky white sneakers (`w_007`)
* **Accessories:** Black crossbody bag (`w_010`)
* **Why it works:** The relaxed fit of the oversized grey crewneck contrasts nicely with the straighter, classic cut of the 501s. Finished off with chunky white sneakers, this gives you a classic, effortless off-duty streetwear vibe.

### Outfit 2: Edgy & Cropped Silhouette
*Play with proportions by pairing a fitted base with a cropped layer and rugged boots.*
* **Top:** White ribbed tank top (`w_003`) layered under the Black cropped zip hoodie (`w_005`)
* **Bottoms:** Vintage Levi's 501 Jeans — Medium Wash (`lst_001`)
* **Shoes:** Black combat boots (`w_008`)
* **Accessories:** Brown leather belt (`w_009`)
* **Why it works:** Tucking in the white tank and wearing the black zip hoodie cropped highlights the waist of the 501s. Adding the brown belt and black combat boots brings in a touch of edge while keeping the color palette grounded and versatile.


```

```
$ python -c "from tools import create_fit_card; ..."

Nothing beats the effortless 90s vibe of a broken-in pair of Vintage Levi's 501 Jeans with just the right amount of knee fading. Style them with your favorite beat-up sneakers and an oversized tee for the ultimate casual weekend fit. Grab this medium wash staple right now on depop for just $38.0! ✨👖
```

---

## How I Used AI

<!-- Two specific moments. What you asked, what came back, what you changed.

     "I used Claude to help me code" is not enough.

     "I gave Claude my search_listings spec. It returned None on no match
     instead of an empty list, so I changed it" is the level we want. -->

**Moment 1**

- **What I asked for:** I asked AI to help diagnose why my `search_listings` terminal test was failing with a `startswith` error.
- **What came back:** AI helped trace the error to `_size_tokens()`, where `.upper` was being used without parentheses, causing method objects instead of strings to be stored.
- **What I changed:** I changed `.upper` to `.upper()` and reran the terminal test to confirm that `search_listings` returned matching listings successfully.

**Moment 2**

- **What I asked for:** I asked AI to help me verify that my agent was actually branching when a search returned no results instead of always calling all three tools.
- **What came back:** AI suggested tracing each tool call and checking that an impossible query stopped after `search_listings` while a matching query continued through all three tools.
- **What I changed:** I added trace calls to the agent and tested both paths. The matching query reached `search_listings`, `suggest_outfit`, and `create_fit_card`, while the impossible query stopped after `search_listings`.

<!-- ═══════════════════════ UNIT 4 — THE TEST ═══════════════════════

     Don't fill these in during unit 3.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Run Log — Before

<!-- Five criteria, five tries each, in this exact format.

     Five, because your criteria are written out of five. Mark each try PASS
     or FAIL, count the passes, and read that count against your target — a
     row targeting 4 of 5 with three PASS cells is MISSED (3/5).

     `python run_eval.py --label before` runs everything and writes the table
     into results/. Paste it here and fill in the verdicts. -->

| Criterion | Target | Try 1 | Try 2 | Try 3 | Try 4 | Try 5 | Verdict |
|---|---|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |  |  |
| 2.  |  |  |  |  |  |  |  |
| 3.  |  |  |  |  |  |  |  |
| 4.  |  |  |  |  |  |  |  |
| 5.  |  |  |  |  |  |  |  |

**Real output from one try**, pasted as text, naming the file and function
that produced it:

```

```

---

## Verdicts and Diagnoses

<!-- MET or MISSED per criterion against LAST UNIT's target, plus a sentence on
     how you decided.

     Then, for every miss: which of the four places it happened — a tool, the
     loop's branch, the session, or the model's output — AND the mechanism.

     Not a diagnosis:  "The fit card was bad."
     A diagnosis:      "The fit card criterion missed on 2 of 5 items. Both had
                        an empty brand field. My prompt puts the brand in the
                        first sentence, so the card opened with a blank and read
                        like a fragment. The tool worked; the prompt assumed a
                        field that isn't always there."

     Look for a pattern. Three misses on the same tool is one problem, not
     three. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**



---

## Loop Trace

<!-- One full run, printed step by step, with the MCP call visible in it.

     `python app.py ask '...' --trace` once you've added the trace.step()
     calls in Milestone 2.

     Worth pasting BOTH the happy path and the empty-search path. The empty
     one should be visibly shorter, because it stops. If your two traces are
     the same length, your branch isn't working — and this is the fastest way
     anyone will ever find that out. -->

**Happy path**
[1] search_listings
    in: dict with keys: description, size, max_price
    out: matching listings
[2] suggest_outfit
    in: selected item and wardrobe
    out: outfit suggestion
[3] create_fit_card
    in: outfit suggestion and selected item
    out: fit card

```

```

**Empty search**
[1] search_listings
    in: dict with keys: description, size, max_price
    out: [] (empty)
    -> branch: no results, stopping

```

```

**On the MCP move:** <!-- what changed in your code, and whether anything
behaved differently afterwards. If the rewire didn't work, say exactly where it
broke — the error text and the last thing that worked. That earns the point in
full. -->

I kept `search_listings` as a local tool for this milestone. The planning loop now searches listings first, stops early when no results are found, and only calls `suggest_outfit` and `create_fit_card` when a matching item exists.

---

## The Improvement

<!-- What you changed, why your diagnosis pointed at it, and the after-run in
     the same table format. One change, measured properly.

     `python run_eval.py --label after` -->

**What I changed:**

**Which failure it was meant to fix:**

### Run Log — After

| Criterion | Target | Try 1 | Try 2 | Try 3 | Try 4 | Try 5 | Verdict |
|---|---|---|---|---|---|---|---|
| 1. Matching query completes all three tools | 4/5 | PASS | PASS | PASS | PASS | PASS | MET (5/5) |
| 2. Impossible query stops before the second tool | 5/5 | PASS | PASS | PASS | PASS | PASS | MET (5/5) |
| 3. Selected item remains the same between tools | 5/5 | PASS | PASS | PASS | PASS | PASS | MET (5/5) |
| 4. Fit card includes important item information | 4/5 | PASS | PASS | PASS | PASS | PASS | MET (5/5) |
| 5. Empty wardrobe still produces useful styling advice | 4/5 | PASS | PASS | PASS | PASS | PASS | MET (5/5) |

**Did it help, and how do I know:**

<!-- If it made things worse, say that. Honestly reported, that earns full
     credit and is more interesting than one that worked. -->

Yes, the evaluation showed that all five criteria met their targets. The matching query completed all three tools, impossible queries consitently stopped early, the selected item remained consistent between tools, the fit card included useful item information, and the empty wardrobe path still produced styling advice.

---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped where
     you did. "I ran out of time" is fine if it's true. Pretending nothing is
     left is not. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 3

       [ ] criteria.md has five numbered criteria, each with a target
       [ ] Each criterion has a reason underneath it
       [ ] All five unit 3 sections above have real content
       [ ] Tool Inventory: all three tools, inputs WITH TYPES, a specific
           return value, and the empty case
       [ ] Planning Loop names the branch rule and agent.py::run_agent
       [ ] Sample Run: one full query plus the three per-tool tests, as text
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN, you submit the same one
           next unit

     SUBMISSION CHECKLIST — unit 4

       [ ] mcp_server.py exists with one tool registered
           (or a written record of exactly where the rewire broke)
       [ ] Run Log — Before, five criteria, five tries each
       [ ] Real output pasted underneath, naming file and function
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, naming a place AND a mechanism
       [ ] Loop Trace, with the MCP call visible in it
       [ ] All three failure modes triggered and handled
       [ ] One improvement, with Run Log — After in the same format
       [ ] What's Still Broken
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository. Your commit history is what
     shows your criteria existed before your results did.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
