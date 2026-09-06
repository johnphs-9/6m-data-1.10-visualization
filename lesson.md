# 📚 Lesson 1.10: Data Visualisation & Storytelling

## Session Overview

| | |
|---|---|
| **Duration** | 180 minutes (including 2 × 10-min breaks) |
| **Format** | Flipped Classroom + Guided Coding in Jupyter |
| **Tools** | VS Code + `pds` conda environment |
| **Notebooks** | `notebooks/Part_1_three_pillars.ipynb` → `Part_4_chart_choice.ipynb` (4 parts, see below) |
| **Dataset** | The Daily Grind café chain — the same files and findings as Lesson 1.9 |

## Agenda

| Time | Part | Topic |
|------|------|-------|
| 0:00 – 0:05 | Setup | Imports, load the data + the 1.9 decision table, the "three versions" hook |
| 0:05 – 0:45 | Part 1 | The Three Pillars — perception, design, storytelling; fix a bad chart in five moves *(incl. 10-min Group Exercise 1)* |
| 0:45 – 0:55 | ☕ | **Break** |
| 0:55 – 1:40 | Part 2 | Matplotlib — Figure → Axes → plot, subplots, ticks, annotation, saving *(incl. 12-min Group Exercise 2)* |
| 1:40 – 1:50 | ☕ | **Break** |
| 1:50 – 2:35 | Part 3 | Seaborn — distributions, box/violin, scatter, heatmap, small multiples *(incl. 12-min Group Exercise 3)* |
| 2:35 – 3:00 | Part 4 | Chart choice, the owner's slide, and the 3-act script |

**The notebook follows the four learning outcomes in order.** Principles come before syntax on
purpose: once you can say *why* a chart is bad, the matplotlib API stops feeling arbitrary.

---

## 🎬 Why this matters — the same finding, three ways

Three cells, in this order:

1. `monthly.round(0).tail(12)` — the finding as a **table**. Complete, precise, unreadable at speed.
2. `monthly.plot()` — the finding as a **default chart**. Five colours, no title, no point.
3. The **built** chart — Marina Bay red, Holland Village green, the other three grey, the competitor
   date annotated, and the finding as the title.

Then ask yourself: *which one makes the owner do something?* Nothing about the data changed between
them. Four things about the drawing did, and they are the four things this whole lesson teaches:

- one sentence as the title,
- colour used as an argument (two lines carry the point; three went grey),
- the cause annotated on the chart itself,
- clutter removed.

> The grey lines are worth ten seconds on their own. They still supply the context that makes the red
> line mean something — without them you cannot see that the other outlets are steady. **Grey is how
> you keep context without spending attention on it.**

---

## 🧭 The one rule this session is built on

> **The chart is chosen by the message, not by taste.**

Write the sentence before you write the code. Every failure in Part 1 comes from doing it the other
way round.

### The cells worth slowing down on

- **Part 1 is where the learning is; Parts 2–3 are the tools.** If you only remember one thing from
  today, make it the five-move fix in 1.3.
- **The truncated-axis pair in 1.2.** Same four numbers, two titles: *"Marina Bay has collapsed to
  nothing!"* and *"Marina Bay earns about two-thirds of what Raffles Place does"*. Ask yourself which
  one you have seen in a real meeting. Then the rule and its reason: **bars encode length, so they
  start at zero; lines encode position and are read for change, so they may be truncated.**
- **Time yourself on the pie chart.** "Which is the second-biggest daypart for Tampines Mall?" — pie
  versus bar. Pie charts are not banned; you just need to know when angle-judging fails.
- **The five-move fix (1.3) is the skill you are assessed on.** Bad chart → fix one thing at a time →
  name the pillar each move serves. You should be able to do this on any chart afterwards.
- **Use `fig, ax = plt.subplots()` from your very first chart.** Starting with `plt.plot()` leaves you
  confused about which chart you are modifying for the rest of the course.
- **`sharey=True` (2.2) is an ethics point, not a styling one.** Without it, small multiples mislead
  by accident.
- **The histogram in 3.1.** The mean ticket lands in the *dip* between two humps, so it describes
  almost no real customer. Strongest argument for distributions you will see.
- **The identical box plots in 3.2 are a finding, not a dead end.** They rule out "Marina Bay's
  customers spend less" in one chart. An absence of difference is a result.
- **Part 4 is the payoff.** The owner's slide is what everything before it was for.
- Deep dives (colour theory, accessibility, Gestalt, `pairplot`, `jointplot`, rcParams, interactive
  libraries) live in `reference.md` for self-study.

---

## 🎯 Learning Objectives

By the end of this session, you will be able to:

1. Apply the three storytelling pillars (Perception, Design, Storytelling) to evaluate and improve visualisations.
2. Create charts using Matplotlib's Figure-Axes-Plot hierarchy with proper customisation.
3. Use Seaborn to produce statistical graphics appropriate for different data types and questions.
4. Select the right visualisation type for a given analytical context and audience.

---

## Before You Start

**Have you completed the pre-class reading?**
- ✓ You can explain pre-attentive processing in one sentence
- ✓ You know the golden rule (bar charts start at zero) **and why**
- ✓ You can name three common chart mistakes
- ✓ You have thought about who the audience is
- ✓ `pds` conda environment is set up

**You will also need your Lesson 1.9 output.** The notebook loads `data/lesson19_decision.csv`. A copy
is already in `data/` so you are not blocked, but if you ran 1.9 yourself, compare the two.

Open `notebooks/Part_1_three_pillars.ipynb` in VS Code and select the `pds` kernel. Each part
notebook ends with a pointer to the next one (`Part_2_matplotlib_fundamentals.ipynb`,
`Part_3_seaborn.ipynb`, `Part_4_chart_choice.ipynb`).

---

## 🏃 Part 1: The Three Pillars (40 min)

Follow **Part 1** in the notebook.

**1.1 Perception — what the eye does before you think.**

| Fact | Consequence for you |
|---|---|
| Colour, length and position register in ~200 ms, pre-attentively | one contrasting element is found instantly; ten are found never |
| Working memory holds about four things | every extra line, colour and label spends a fixed budget |

Length along a common baseline is the most accurately judged visual channel; angle and area are among
the worst. That ranking is the whole reason bars beat pies.

**1.2 Design — honest and not.** The truncated axis, then the pie-versus-bar race. See "the cells
worth slowing down on" above; these two cells carry the section.

**1.3 Storytelling — fix a bad chart in five moves.** Start from `by_daypart.plot(kind="bar")` and
list what is wrong before you touch it. Then:

| Move | Pillar |
|---|---|
| 1. Percentages instead of dollars (matches the number to the question) | Design |
| 2. Drop the closed pop-up kiosk | Perception |
| 3. Real outlet names instead of codes | Perception |
| 4. Title states the finding | Storytelling |
| 5. Stacked horizontal bars — each row is one 100% whole | Design |

> Final title: *"Two of these are commuter cafés; one is a neighbourhood café."* Nothing about the
> data changed. The chart went from useless to actionable through presentation alone.

---

## 🏃 Part 2: Matplotlib Fundamentals (45 min)

Continue with **Part 2**.

**2.1 The hierarchy.** Three levels, and almost every error is a confusion between them:

| Level | What it is | Analogy |
|---|---|---|
| Figure | the whole image, the thing you save | the sheet of paper |
| Axes | one panel, one set of x/y axes | a chart on the paper |
| artists | lines, bars, text | the ink |

`fig, ax = plt.subplots()` creates the first two. Everything else hangs off `ax`. The four habits
worth using every time: include zero where magnitude matters, drop the top and right spines, faint
y-grid only, title left-aligned.

**2.2 Subplots.** A 2×2 grid of the four outlets with `sharex` and `sharey`, plus `fill_between`.
Small multiples are the most underrated chart in business reporting.

**2.3 Ticks, limits and labels.** `FuncFormatter` for `$xxk`, and fewer x-ticks so date labels stay
horizontal. Small changes, large readability gain.

**2.4 Annotation.** `axvline`, `annotate` with an arrow, `axvspan` to shade the "after" regime, then
`fig.savefig(..., dpi=150, bbox_inches="tight")`.

> **The point of annotation:** a chart that travels without you has to explain itself. Most business
> charts are forwarded, not presented.

---

## 🏃 Part 3: Seaborn for Statistical Graphics (45 min)

Continue with **Part 3**. Seaborn takes a DataFrame and column *names*, does the grouping itself, and
hands back a matplotlib Axes you can still customise.

- **3.1 Distributions.** `histplot`, `histplot(hue=...)`, `kdeplot`. **The payoff cell:** ticket
  values are two humps — drinks around \$5–7, meals around \$13–18 — and the mean (\$8.38) lands in
  the dip between them. It describes almost no real customer.
- **3.2 Categories.** `boxplot` by outlet (near-identical — a finding by absence), `violinplot` by
  daypart (shows the two humps a box hides), `barplot` (which plots the **mean** and a 95% CI by
  default — remember this one).
- **3.3 Relationships.** `scatterplot`, then `regplot`. The regression line makes the sceptical point:
  revenue *is* tickets × average ticket, so a tight fit here is arithmetic, not insight.
  "Could this have come out any other way?" is the question to keep asking.
- **3.4 Heatmaps and small multiples.** `heatmap(annot=True)` — colour alone cannot be read
  precisely, so print the numbers. Then `relplot(col=...)` for one panel per outlet.

> **Figure-level vs Axes-level** is worth a slow minute. `histplot`, `boxplot`, `scatterplot` accept
> `ax=`. `relplot`, `catplot`, `displot`, `pairplot`, `jointplot` build their own Figure and return a
> grid object — `ax=` raises an error, and you reach matplotlib through `g.figure` / `g.axes`.
> Two-thirds of confusing seaborn errors are this distinction.

---

## 🏃 Part 4: Chart Choice & the One Slide (25 min)

Continue with **Part 4**.

**4.1 The message picks the chart** — the selection table, then the audience table (owner wants the
decision; manager wants the driver; data team wants the method). Same analysis, three framings.

**4.2 The owner's slide.** One Figure, two Axes: the trend that carries the finding (2 parts width)
and the rent share that makes it a decision (1 part). The message is the Figure title. Saved to
`visualisations/owner_slide.png`.

**4.3 The 20-second script**, in three acts:

> **Context:** "Chain revenue has been flat for two quarters, so nothing looks urgent."
> **Finding:** "It is flat because two outlets cancel out — Marina Bay down 28%, Holland Village up
> 27% — and Marina Bay's fall happened in one week last November when a competitor opened next door."
> **Decision:** "Marina Bay now spends 28% of revenue on rent against 15% elsewhere, and earns \$20
> per staff hour against \$25–27. Before you sign, the question is whether that November step can be
> reversed."

> **End on this.** The slide does not say "close Marina Bay". Making the comparison unavoidable was
> your job as the analyst; the decision is the owner's. That line is the difference between an analyst
> and someone with an agenda and a chart.

Finish by running the checklist cell against your own Group Exercise output.

---

## 🎯 Wrap-Up

**Key Takeaways:**
1. The chart is chosen by the message. Write the sentence first.
2. Perception is a budget — spend colour on one thing and grey the rest.
3. Bars start at zero; lines do not have to.
4. The title is the finding, not the metric.
5. Annotate the cause on the chart, so it survives being forwarded.
6. Distributions before averages — an $8.38 mean ticket described a customer who does not exist.
7. An absence of difference is a finding.
8. Small multiples need a shared scale, or they mislead by accident.
9. Your job is to make the comparison unavoidable, not to make the decision.

**Next Steps:**
- Complete the [Assignment](./assignment.md) — critique, redesign, and build your own one slide.
- **Module 1 is complete.** Trust the data (1.8) → find the pattern (1.9) → make them act (1.10).
