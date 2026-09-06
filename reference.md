# Reference — Lesson 1.10

- [Matplotlib official cheatsheets (print these)](https://matplotlib.org/cheatsheets/)
- [Seaborn Cheatsheet](https://www.datacamp.com/cheat-sheet/python-seaborn-cheat-sheet)
- [From Data to Viz — chart chooser with code](https://www.data-to-viz.com/)
- [Financial Times Visual Vocabulary](https://github.com/Financial-Times/chart-doctor/blob/main/visual-vocabulary/poster.png)
- [Data Storytelling Cheatsheet](https://www.datacamp.com/cheat-sheet/data-storytelling-and-communication-cheat-sheet)
- [Color Theory in Logo Design](https://www.logodesign.net/blog/color-theory-in-logo-design/)

---

## 🗺️ The one rule

**The chart is chosen by the message, not by taste.** Write the sentence first.

---

## 📐 Chart chooser

| Your message | Chart | Code |
|---|---|---|
| "X changed over time" | line | `ax.plot(x, y)` |
| "X is bigger than Y" | bar **from zero** | `ax.bar(labels, values)` |
| "X is bigger than Y" (long labels) | horizontal bar, sorted | `ax.barh(labels, values)` |
| "X is made of these parts" | stacked bar | `ax.barh(..., left=cumulative)` |
| "X and Y move together" | scatter | `sns.scatterplot(...)` |
| "…and here is the trend" | scatter + fit | `sns.regplot(...)` |
| "X varies more than you think" | histogram | `sns.histplot(...)` |
| "these groups have different spreads" | box / violin | `sns.boxplot(...)`, `sns.violinplot(...)` |
| "the pattern differs by group" | small multiples | `plt.subplots(2, 2, sharey=True)`, `sns.relplot(col=...)` |
| "this cell is the hot spot" | heatmap | `sns.heatmap(pivot, annot=True)` |
| "one number matters" | write the number | no chart |

**Audience sets the depth, never the truth:**

| Audience | Wants | Chart |
|---|---|---|
| Owner / executive | the decision | one panel, finding as title, cause annotated |
| Manager | what to change | driver broken out, comparison to target |
| Data team | the method | small multiples, distributions, uncertainty |

---

## 🎨 Matplotlib cheat sheet

| Task | Code |
|---|---|
| One panel | `fig, ax = plt.subplots(figsize=(8, 4))` |
| Grid of panels | `fig, axes = plt.subplots(2, 2, sharex=True, sharey=True)` |
| Uneven panel widths | `plt.subplots(1, 2, gridspec_kw={"width_ratios": [2, 1]})` |
| Line / bar / horizontal bar | `ax.plot(...)`, `ax.bar(...)`, `ax.barh(...)` |
| Shaded area under a line | `ax.fill_between(x, y, alpha=0.2)` |
| Titles | `ax.set_title("...", loc="left")`, `fig.suptitle("...")` |
| Axis labels and limits | `ax.set_xlabel()`, `ax.set_ylabel()`, `ax.set_ylim(0, None)` |
| Choose the ticks | `ax.set_xticks(vals)`, `ax.set_xticklabels(text, rotation=0)` |
| Format tick text | `ax.yaxis.set_major_formatter(FuncFormatter(lambda v, _: f"${v/1000:,.0f}k"))` |
| Remove the box | `ax.spines[["top", "right"]].set_visible(False)` |
| Faint horizontal grid | `ax.grid(axis="y", alpha=0.3)` |
| Vertical / horizontal rule | `ax.axvline(x, ls="--")`, `ax.axhline(y)` |
| Shade a range | `ax.axvspan(start, end, alpha=0.1)` |
| Free text | `ax.text(x, y, "words")` |
| Text with an arrow | `ax.annotate("words", xy=(x, y), xytext=(x2, y2), arrowprops=dict(arrowstyle="->"))` |
| Legend without a box | `ax.legend(frameon=False)` |
| Tidy spacing | `plt.tight_layout()` |
| Save | `fig.savefig("f.png", dpi=150, bbox_inches="tight")` |

**The habit:** always `fig, ax = plt.subplots()`, then only ever `ax.something(...)`. The bare
`plt.something(...)` functions act on "whichever chart is current", which is fine for one panel and a
debugging nightmare with several.

---

## 📊 Seaborn cheat sheet

| Task | Code |
|---|---|
| Set the look, once | `sns.set_theme(style="whitegrid", palette="colorblind")` |
| Histogram | `sns.histplot(data=df, x="col", bins=30, ax=ax)` |
| …split by category | `sns.histplot(..., hue="cat", element="step", fill=False)` |
| Smoothed density | `sns.kdeplot(data=df, x="col", hue="cat", fill=True)` |
| Box plot | `sns.boxplot(data=df, x="cat", y="val", order=[...], ax=ax)` |
| Violin plot | `sns.violinplot(data=df, x="cat", y="val", ax=ax)` |
| Bar of group **means** + CI | `sns.barplot(data=df, x="cat", y="val", errorbar=("ci", 95))` |
| Counts per category | `sns.countplot(data=df, x="cat")` |
| Scatter | `sns.scatterplot(data=df, x="a", y="b", hue="cat", alpha=0.5, s=18)` |
| Scatter + fitted line | `sns.regplot(data=df, x="a", y="b")` |
| Heatmap | `sns.heatmap(pivot, annot=True, fmt=".1f", cmap="Blues")` |
| Small multiples (lines) | `sns.relplot(data=df, x="a", y="b", col="cat", col_wrap=2, kind="line")` |
| Small multiples (categories) | `sns.catplot(data=df, x="a", y="b", col="cat", kind="box")` |
| Small multiples (distributions) | `sns.displot(data=df, x="val", col="cat")` |
| Every pair of columns | `sns.pairplot(df[["a", "b", "c"]], corner=True)` |
| Scatter + marginal histograms | `sns.jointplot(data=df, x="a", y="b", kind="hex")` |
| Remove the box | `sns.despine()` |

**Axes-level vs Figure-level — the distinction behind most confusing seaborn errors:**

| | Accepts `ax=` | Returns | Examples |
|---|---|---|---|
| **Axes-level** | yes | the Axes | `histplot`, `boxplot`, `violinplot`, `scatterplot`, `regplot`, `heatmap`, `barplot`, `kdeplot` |
| **Figure-level** | **no** | a grid object | `relplot`, `catplot`, `displot`, `pairplot`, `jointplot`, `lmplot` |

With a Figure-level function, reach matplotlib through `g.figure` and `g.axes`:

```python
g = sns.relplot(data=df, x="date", y="revenue", col="outlet_id", col_wrap=2, kind="line")
g.set_titles("{col_name}")
g.figure.suptitle("One panel per outlet", y=1.03)
g.figure.savefig("panels.png", dpi=150, bbox_inches="tight")
```

---

## ✅ Pre-send checklist

1. Does the title state the **finding**, not the metric?
2. Do bar charts start at **zero**?
3. Is colour carrying **meaning** rather than decoration?
4. Are there fewer than about **five** things demanding attention?
5. Do axis labels include **units**?
6. Would it survive being printed in **black and white**?
7. If the reader saw only this chart, with no voiceover, would they get the message?

---

## 📦 Moved out of the lesson notebook

All of this runs. Blocks assume:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

sales = pd.read_csv("../data/daily_sales.csv", parse_dates=["date"])
outlets = pd.read_csv("../data/outlets.csv")
tickets = pd.read_csv("../data/tickets_week.csv", parse_dates=["txn_datetime"])
names = outlets.set_index("outlet_id")["outlet_name"].to_dict()
daily = sales.groupby(["date", "outlet_id"], as_index=False).agg(
    revenue=("revenue_sgd", "sum"), tickets=("tickets", "sum")
)
monthly = sales.pivot_table(
    index="date", columns="outlet_id", values="revenue_sgd", aggfunc="sum"
).resample("M").sum()
```

### `pairplot` — every pair of columns at once

```python
# One panel per pair of columns. `corner=True` drops the redundant mirror half.
g = sns.pairplot(monthly[["OUT-01", "OUT-02", "OUT-03", "OUT-04"]], corner=True, height=1.8)
g.figure.suptitle("Monthly revenue, every pair of outlets", y=1.02)
plt.show()
```

> Useful for exploration, almost never for presentation: sixteen panels is four times the attention
> budget. Use it to *find* the pair worth showing, then show that pair on its own.

### `jointplot` — a scatter with its margins

```python
# `kind="hex"` bins the points, which stops overplotting hiding the density.
g = sns.jointplot(data=daily[daily["outlet_id"] == "OUT-01"],
                  x="tickets", y="revenue", kind="hex", height=4.5)
g.figure.suptitle("Raffles Place: tickets vs revenue", y=1.02)
plt.show()
```

### `FacetGrid` — small multiples you control by hand

```python
# `relplot`/`catplot` are wrappers over this. Use FacetGrid when you need a custom draw.
d = daily[daily["outlet_id"].isin(["OUT-01", "OUT-02", "OUT-03", "OUT-04"])].copy()
d["weekend"] = d["date"].dt.dayofweek >= 5

g = sns.FacetGrid(d, col="outlet_id", hue="weekend", col_wrap=2, height=2.4, aspect=1.6)
g.map(sns.scatterplot, "tickets", "revenue", alpha=0.4, s=12)
g.add_legend(title="weekend")
g.set_titles("{col_name}")
plt.show()
```

### Styles, palettes and rcParams

```python
# The five built-in styles, one small chart each. `with sns.axes_style(...)` applies a style
# to just the charts created inside the block, so your global setting is left alone.
for style in ["white", "dark", "whitegrid", "darkgrid", "ticks"]:
    with sns.axes_style(style):
        fig, ax = plt.subplots(figsize=(3.2, 1.6))
        ax.plot([1, 3, 2, 4])
        ax.set_title(style, fontsize=9)
        plt.show()
```

```python
# Palette types, and when each is right.
for name, kind in [("colorblind", "qualitative -- unordered categories"),
                   ("Blues", "sequential -- low to high"),
                   ("RdBu_r", "diverging -- has a meaningful middle")]:
    print(f"{name:<12} {kind}")
    sns.palplot(sns.color_palette(name, 7))
    plt.show()
```

```python
# House style in one place: set it once at the top of a notebook, forget about it.
plt.rcParams.update({
    "figure.figsize": (8, 4),
    "figure.dpi": 110,
    "axes.spines.top": False,
    "axes.spines.right": False,
    "axes.titlesize": 12,
    "axes.titleweight": "bold",
    "axes.titlelocation": "left",
    "font.size": 10,
    "legend.frameon": False,
})

fig, ax = plt.subplots()
ax.plot(monthly.index, monthly.sum(axis=1))
ax.set_title("Every chart now starts like this")
plt.show()
```

### Saving for screen and for print

```python
fig, ax = plt.subplots(figsize=(7, 3.5))
ax.plot(monthly.index, monthly["OUT-03"], color="#c0392b", linewidth=2)
ax.set_title("Marina Bay")

import os
os.makedirs("../visualisations", exist_ok=True)

fig.savefig("../visualisations/ref_screen.png", dpi=110, bbox_inches="tight")   # slides, email
fig.savefig("../visualisations/ref_print.png", dpi=300, bbox_inches="tight")    # print
fig.savefig("../visualisations/ref_vector.svg", bbox_inches="tight")            # scales forever
plt.show()

print(sorted(f for f in os.listdir("../visualisations") if f.startswith("ref_")))
```

> **Raster (`png`, `jpg`)** stores pixels, so it blurs when enlarged — fine at the right size.
> **Vector (`svg`, `pdf`)** stores instructions, so it is sharp at any size and is what you want for
> anything printed or projected. `bbox_inches="tight"` is what stops your axis labels being cut off.

### A dual axis, and why to avoid it

```python
# Two different units on one chart. It works, and it is almost always a bad idea.
fig, ax1 = plt.subplots(figsize=(8, 3.6))
m3 = monthly["OUT-03"]

ax1.plot(m3.index, m3.values, color="#c0392b")
ax1.set_ylabel("revenue (S$)", color="#c0392b")

ax2 = ax1.twinx()                       # 👈 a second y-axis sharing the same x
ax2.plot(m3.index, m3.values / 8.1, color="#2980b9")
ax2.set_ylabel("tickets (approx)", color="#2980b9")

ax1.set_title("Two axes: the shape of 'agreement' is whatever you scale it to")
plt.show()
```

> Whoever sets the two scales decides how strongly the lines appear to agree, so a dual axis can
> manufacture a relationship out of nothing. Prefer two stacked panels sharing an x-axis, or index
> both series to 100 at the start and plot them on one scale.

---

## 🔬 Deep dive: the research behind the pillars

### Pre-attentive processing

- Human vision processes colour, shape and position **in parallel**, before conscious attention
  (Treisman, 1985). This takes ~200–500 ms; deliberate search of a table takes seconds.
- **Implication:** one or two pre-attentive cues per chart. A third competes with the first two rather
  than adding to them.

### The accuracy ranking (Cleveland & McGill, 1984)

Ranked most to least accurately judged:

1. position along a common scale
2. position on identical, non-aligned scales
3. length
4. angle / slope
5. area
6. volume, colour saturation

Bar and dot charts use ranks 1–3. Pie charts use rank 4. Bubble charts use rank 5. Nothing sensible
uses rank 6.

### Gestalt principles — how the eye groups things

| Principle | Meaning | Use it to |
|---|---|---|
| Proximity | close together = related | group related panels; add space between unrelated ones |
| Similarity | same colour/shape = same group | colour a category consistently across every chart |
| Continuity | the eye follows smooth paths | keep a line unbroken; avoid crossing gridlines |
| Closure | the brain completes shapes | drop the chart border — the axes imply it |
| Enclosure | a shared box = a group | shade the "after competitor" period |

---

## 🎨 Deep dive: colour and accessibility

### Three kinds of palette

| Type | For | Examples |
|---|---|---|
| **Qualitative** | unordered categories | `colorblind`, `Set2`, `tab10` |
| **Sequential** | low → high | `Blues`, `viridis`, `YlGnBu` |
| **Diverging** | a meaningful midpoint (±, above/below target) | `RdBu_r`, `PiYG`, `coolwarm` |

Using a qualitative palette for ordered data is a common error: the reader cannot tell which colour is
"more" without consulting the legend on every glance.

### Accessibility

- Around **8% of men** have red–green colour vision deficiency. A red-versus-green chart excludes a
  meaningful part of any large audience.
- **Never rely on colour alone.** Add direct labels, line styles or annotation, so the chart still works
  in greyscale — which is also how it will look when printed.
- `viridis` and seaborn's `colorblind` palette are perceptually uniform and colour-blind-safe. This is
  why `viridis` exists, and why it is worth typing.
- Aim for a text/background contrast ratio of at least **4.5:1**.

**Tools:** [Coblis colour-blindness simulator](https://www.color-blindness.com/coblis-color-blindness-simulator/) ·
[Paul Tol's palettes](https://personal.sron.nl/~pault/colormaps.html) ·
[WebAIM contrast checker](https://webaim.org/resources/contrastchecker/)

---

## ⚖️ Deep dive: the four ways charts mislead

| Trick | What it does | Fix |
|---|---|---|
| Truncated bar axis | a 5% gap looks like 50% | bars start at zero |
| 3D effects | perspective distorts area and angle | 2D, always |
| Dual axes | the apparent correlation is whatever you scaled it to | two panels, or index both to 100 |
| Chart junk | spends attention on nothing | delete anything that does not explain the data |

**Four questions before you send:** Is it accurate? Will the audience read it the way I intend? Could a
reasonable person read the opposite conclusion out of it? Can someone with colour-blindness or low
vision use it?

---

## 📖 Going further

**Books**

- *Storytelling with Data*, Cole Nussbaumer Knaflic — the most practical book on this topic; chapter 1
  is free online.
- *The Visual Display of Quantitative Information*, Edward Tufte — the classic; the source of "chart junk".
- *Visualization Analysis & Design*, Tamara Munzner — rigorous, for when you want the theory.

**Blogs and galleries**

- [Flowing Data](https://flowingdata.com/) — practical and consistently good.
- [The Pudding](https://pudding.cool/) — the standard for visual storytelling.
- [Storytelling with Data blog](https://www.storytellingwithdata.com/blog) — regular before/after makeovers.

**Beyond matplotlib**

- **Plotly / Bokeh** — interactive charts where hovering reveals detail. Worth it when the audience
  needs to *explore*; unnecessary when you need them to *understand one thing*.
- **Altair** — declarative grammar-of-graphics; concise for complex layered charts.
- **Dash / Streamlit** — full dashboards. Remember the dashboard rule: 3–5 metrics, most important
  largest, and a visible timestamp.

> **Static tells a story; interactive lets them explore.** Choose by purpose, not by novelty. The
> owner deciding on a lease in twenty seconds does not want to filter anything.
