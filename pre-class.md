# 📚 Pre-Class: Making Data Speak

⏱️ **Estimated Time:** 35–40 minutes
**Prerequisites:** Lesson 1.9 — EDA Advanced

> In Lesson 1.9 you found the answer: the café chain's flat revenue is Marina Bay collapsing while
> Holland Village covers it up, and Marina Bay's fall is a step dated to the first week of November
> 2024. You have the table. **Now you have twenty seconds of somebody's attention.**
>
> This lesson is about that twenty seconds. It is not decoration, and it is not the soft part of the
> module — it is the part where analysis either changes a decision or doesn't.

🎯 **Goal:** get the design vocabulary before the code, so every plotting choice in class has a reason
behind it.

🎬 Watch this video: [[L1.10 The 20 Second Window  Hacking Human Perception with Data]](https://youtu.be/YWIlp4dG8FY)

---

## **0. The one rule (2 minutes)**

> **The chart is chosen by the message, not by taste.**

Write the sentence you want the audience to leave with, in words, *before* you touch any plotting code.
Then the chart type follows almost mechanically:

| The sentence you want them to leave with | The chart |
|---|---|
| "X changed over time" | line |
| "X is bigger than Y" | bar, starting at zero |
| "X is made up of these parts" | stacked bar |
| "X and Y move together" | scatter |
| "X varies more than you think" | box, violin or histogram |
| "the pattern differs by group" | small multiples with shared axes |

Almost every bad chart you have seen was made the other way round: someone picked a chart type, then
went looking for something to say with it.

---

## **1. Pillar 1 — Perception: how humans see (10 minutes)**

### Pre-attentive processing

Your visual system registers colour, length and position in roughly **200–500 milliseconds** — *before*
conscious attention arrives. One red bar among grey ones is found instantly and effortlessly.

The practical rule: **use colour for the one thing that matters, and grey for everything else.** In
class you will see the same five-line chart drawn twice — once with five bright colours, once with one
red line and four grey. The second answers "which outlet is failing?" before you have read the title.

> **Grey is not "hiding" data.** The grey lines still provide the context that makes the red one
> meaningful — without them you cannot tell whether the red line is unusual. Grey keeps context without
> spending attention on it.

### Cognitive load

Working memory holds about **four items** at once. Every extra line, colour, gridline and label spends
part of a fixed budget. Removing clutter is not an aesthetic preference; it is buying back attention for
the thing you want noticed.

### The accuracy ranking

Humans judge some visual channels far more accurately than others:

**position ≈ length ≫ angle ≈ area ≫ colour intensity ≫ volume**

That single ranking explains most chart advice you will ever be given — including why bar charts beat
pie charts, and why nothing good has ever come from a 3D exploding donut.

---

## **2. Pillar 2 — Design: showing data honestly (10 minutes)**

### The golden rule, and *why* it exists

❌ **Never truncate the axis of a bar chart.**
✅ **Bars start at zero.**

The reason matters more than the rule. A bar encodes its value as **length**. If the axis starts at
180,000 instead of 0, the length is no longer proportional to the value, and the reader's eye is doing
arithmetic on a false premise. In class you will see the same four numbers drawn twice: truncated,
Marina Bay looks like it earns about a tenth of Raffles Place. It earns **66%** of it.

**Line charts are different.** A line encodes *position* and is read for *change*, so truncating is
often correct — it lets a real 3% movement be visible instead of flattened into a straight line.

> **Bars start at zero. Lines start wherever the change is legible.**

### Five mistakes to recognise

| Mistake | Why it fails | Instead |
|---|---|---|
| Truncated bar axis | length no longer matches value | start at zero, or use a line/dot chart |
| Pie chart with 6+ slices | angle-judging is inaccurate | horizontal bar chart, sorted |
| Eight colours | blows the attention budget | one accent colour, the rest grey |
| Title names the metric ("Monthly Revenue") | tells the reader nothing | title states the finding |
| 3D anything | perspective distorts area and angle | 2D, always |

---

## **3. Pillar 3 — Storytelling: three acts (8 minutes)**

A chart alone is evidence, not an argument. Wrap it in three sentences:

```
ACT 1 — CONTEXT      "Why should you care?"
ACT 2 — FINDING      "Here is what the data shows"   <- the chart lives here
ACT 3 — DECISION     "So here is the choice in front of you"
```

Applied to the café chain — this is the actual script you will build in class:

- **Act 1:** "Chain revenue has been flat for two quarters, so nothing looks urgent."
- **Act 2:** "It is flat because two outlets cancel out. Marina Bay is down 28% year on year and
  Holland Village is up 27%. Marina Bay's fall happened in one week last November, when a competitor
  opened next door."
- **Act 3:** "Marina Bay now spends 28% of its revenue on rent against 15% everywhere else. Before you
  sign the lease, the question is whether that November step can be reversed."

**Notice what Act 3 does not do.** It does not say "close Marina Bay". Your job is to make the
comparison unavoidable; the decision belongs to the person carrying the risk. That distinction is what
separates an analyst from someone with an agenda and a chart.

---

## **4. Know your audience (5 minutes)**

Same analysis every time. Only the framing changes.

| Audience | Cares about | So the chart is | Detail |
|---|---|---|---|
| **Owner / executive** | the decision | one panel, finding as the title, cause annotated | high-level only |
| **Manager** | what to change | the driver broken out, comparison to target | key drivers |
| **Data team** | the method | small multiples, distributions, uncertainty shown | full |
| **General public** | why it affects them | one simple comparison, no jargon | minimal |

Showing an executive the chart you built for the data team is the most common failure in this lesson,
and it reads as "you did not think about me".

---

## **5. Preparation checklist (2 minutes)**

1. **Environment:** the `pds` conda environment. If it is not set up: `conda env create -f environment.yml`
   then `conda activate pds`.
2. **Check it works:** `import matplotlib.pyplot as plt; import seaborn as sns` — no errors means you
   are ready.
3. **Data:** already in `data/`, carried over from Lesson 1.9 along with your decision table.
4. **Mindset:** several cells in class are deliberately bad charts. This is the one topic where
   breaking things on purpose is the fastest way to learn.

---

## **🤖 AI Companion Exercise (recommended)**

**Prompt 1 (critique):** "I have a bar chart of revenue for four shops where the y-axis starts at
150,000 instead of 0. Explain to me, as if I were presenting it to a business owner, exactly how this
misleads them and what they would wrongly conclude."

**Prompt 2 (chart choice):** "I want to show that one of our four shops declined sharply after a
specific date, while the others stayed flat. Suggest three chart options, and for each one say what it
makes obvious and what it hides."

**Prompt 3 (titles):** "Rewrite these chart titles so each states a finding rather than naming a
metric: 'Monthly Revenue', 'Revenue by Outlet', 'Ticket Value Distribution'. Explain what information
you had to invent to do it, and what that tells me about writing good titles."

---

## **🧠 Quick Self-Check**

Answer without scrolling up, then check below.

1. What is pre-attentive processing, and what one design habit follows from it?
2. Why must bar charts start at zero when line charts do not have to?
3. Your chart is titled "Revenue by Outlet". What is wrong with that, and what would you write instead?
4. You have twelve shops to compare over eighteen months. Twelve lines on one chart, or twelve small
   panels? Why?
5. Your audience is one person — the owner deciding on a lease. What do you cut from the chart you would
   have shown your data-team colleagues?

<details>
<summary>💡 Suggested answers</summary>

**Q1:** Your visual system registers colour, length and position in about 200–500ms, before conscious
attention. The habit that follows: use one accent colour for the single thing that matters and grey for
everything else, so the point is found before it is read. A chart with five bright colours has no
pre-attentive channel left — nothing stands out, so nothing is found early.

**Q2:** Because they encode value differently. A bar means "this length **is** this value", so a
non-zero baseline makes the length dishonest. A line means "this is the position at this time", and it
is read for *change* — so a zoomed axis is often what makes a real 3% move visible instead of a flat
line. Bars: length, start at zero. Lines: position, truncate as needed for legibility.

**Q3:** It names the metric instead of stating the finding, so the reader has to work out the point for
themselves — and readers who are skimming will not. Write the sentence you want them to leave with:
"Marina Bay is falling and Holland Village is hiding it". If you cannot write that sentence, you do not
yet know what the chart is for, which is a very useful thing to discover before the meeting rather than
during it.

**Q4:** Twelve small panels (small multiples), with a **shared y-axis**. Twelve lines on one set of axes
is a spaghetti chart: it exceeds the attention budget and no single shop is followable. Small multiples
make the *shapes* comparable at a glance. The shared axis is essential — without it each panel is scaled
to its own data, and a small shop looks the same size as a large one.

**Q5:** Cut the uncertainty bands, the distributions, the methodology panels and the extra dimensions.
Keep one chart, a title that states the finding, the cause annotated on it, and the one comparison that
frames the decision (here: rent as a share of revenue). Nothing is being hidden — the detail still
exists in your notebook and you bring it if asked. You are matching the depth to the decision.

</details>

---

## **Reference**

- [Data Visualization Introduction (Tableau)](https://www.tableau.com/learn/articles/data-visualization)
- [Best Practices for Effective Data Visualization](https://www.thoughtspot.com/data-trends/data-visualization/best-practices-and-tips-for-effective-data-visualization)
- [From Data to Viz — a chart chooser with code](https://www.data-to-viz.com/)
- [Financial Times Visual Vocabulary — a one-page chart chooser, worth printing](https://github.com/Financial-Times/chart-doctor/blob/main/visual-vocabulary/poster.png)

Deeper material — colour theory, accessibility, Gestalt principles and the research behind
pre-attentive processing — is in [reference.md](./reference.md). It is optional, and genuinely
interesting once the basics are in place.
