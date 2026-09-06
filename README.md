# 📚 Lesson 1.10: Data Visualisation & Storytelling

**Theme:** Make Them Act — the chart is chosen by the message, not by taste

---

## 📅 Lesson Overview

**Total: 180 minutes**, including 2 × 10-minute breaks and one group exercise per section.

| Section | Duration | Topic / Activity |
|---------|----------|-----------------|
| Setup | 5 min | Imports, load the café data + your 1.9 decision table, the "three versions" hook |
| **Part 1: The Three Pillars** | 40 min | Perception, design, storytelling; fix a bad chart in five moves *(incl. 10-min Group Exercise 1)* |
| ☕ Break | 10 min | |
| **Part 2: Matplotlib Fundamentals** | 45 min | Figure → Axes → plot; subplots; ticks & limits; annotation; saving *(incl. 12-min Group Exercise 2)* |
| ☕ Break | 10 min | |
| **Part 3: Seaborn** | 45 min | Distributions, box/violin, scatter & regression, heatmaps, small multiples *(incl. 12-min Group Exercise 3)* |
| **Part 4: Chart Choice & the One Slide** | 25 min | Message → chart table; build the owner's slide; the 3-act script |

**One message, start to finish.**

> Same business problem as Lesson 1.9 — **The Daily Grind** café chain. You already know the answer:
> chain revenue is flat because Marina Bay is collapsing and Holland Village is covering it up, and
> Marina Bay's fall is a step dated to the first week of November 2024.
>
> This lesson is the last twenty seconds — the part where the owner either acts on your analysis or
> doesn't. Everything here is drawn from the tables you built in 1.9.

The session opens by showing the same finding three ways: as a table of numbers, as a chart made
without thinking, and as a chart made to carry one point. Nothing about the data changes between
them. Only one of them makes somebody do something.

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

1. **Apply** the three storytelling pillars (Perception, Design, Storytelling) to evaluate and improve visualisations.
2. **Create** charts using Matplotlib's Figure-Axes-Plot hierarchy with proper customisation.
3. **Use** Seaborn to produce statistical graphics appropriate for different data types and questions.
4. **Select** the right visualisation type for a given analytical context and audience.

---

## 🧭 The one rule

> **The chart is chosen by the message, not by taste.**

Write the sentence you want the audience to leave with, in words, *before* you touch any plotting
code. The chart type then follows almost mechanically:

| The sentence | The chart |
|---|---|
| "X changed over time" | line |
| "X is bigger than Y" | bar, from zero |
| "X is made of these parts" | stacked bar |
| "X and Y move together" | scatter |
| "X varies more than you think" | box, violin, histogram |
| "the pattern differs by group" | small multiples, shared axes |
| "this cell is the hot spot" | heatmap |

Every mistake in Part 1 comes from working the other way round: picking a chart, then hunting for
something to say with it.

---

## 📂 Course Materials

| Material | Description | Est. Time |
|----------|-------------|-----------|
| [Pre-Class](./pre-class.md) | The three pillars; the golden rule; common mistakes; audience | 30–40 min |
| [Lesson Plan](./lesson.md) | Instructor guide: agenda, timings, teaching notes | 180 min |
| [Assignment](./assignment.md) | Critique, redesign, and build your own one slide | 75–90 min |
| [Reference](./reference.md) | Matplotlib & seaborn cheat sheet; chart chooser; colour & accessibility deep dive | As needed |

---

## 🗂️ The Data

Carried over from Lesson 1.9, so nothing here needs re-deriving.

| File | Grain | Used for |
|---|---|---|
| `daily_sales.csv` | outlet × day × daypart | the trend and mix charts |
| `outlets.csv` | one row per outlet | real names for labels, rent, seats |
| `tickets_week.csv` | one row per till receipt (one week) | distributions — where averages fall apart |
| `lesson19_decision.csv` | one row per outlet | the decision table you produced in 1.9 |
| `lesson19_monthly_by_outlet.csv` | month × outlet | the monthly table you produced in 1.9 |

`tips.csv`, `spx.csv` and `macrodata.csv` are left in `data/` from the earlier version of this lesson,
in case you want extra practice data. Nothing in the notebook, assignment or reference uses them —
the lesson runs entirely on the café spine.

**Outputs.** Part 2 and Part 4 save PNGs into `visualisations/` — including `owner_slide.png`, the
figure the whole session builds towards.

---

## 🛠️ Tools & Setup

- **[VS Code](https://code.visualstudio.com)** + Python + Jupyter extensions *(recommended)*.
- **[Google Colab](https://colab.research.google.com)** *(alternative)*.
- **Notebooks:** the lesson is split into four self-contained parts — open them in order,
  selecting the `pds` kernel in VS Code:
  1. `notebooks/Part_1_three_pillars.ipynb`
  2. `notebooks/Part_2_matplotlib_fundamentals.ipynb`
  3. `notebooks/Part_3_seaborn.ipynb`
  4. `notebooks/Part_4_chart_choice.ipynb`
  
  (The original single notebook is kept for reference at `notebooks/_original/data_visualization_lesson.ipynb`.)
- **Environment:** `conda env create -f environment.yml` then `conda activate pds`.
- **Versions:** written for matplotlib 3.7 / seaborn 0.12 (the `pds` environment) and tested on
  newer versions too, so it also runs on Colab.

---

## ➡️ Where this sits

| Lesson | The question it answers |
|---|---|
| 1.8 EDA Basic | **Can I trust this data?** |
| 1.9 EDA Advanced | **What is the pattern?** |
| **1.10 Visualisation** | **How do I make them act?** |

That is the whole arc of Module 1: take a messy file, find the pattern in it, and make someone act on
it. After this lesson you have done all three on one real problem.

---

## Quick Comparison: Matplotlib, Pandas Plotting, Seaborn

| | **Matplotlib** | **Pandas `.plot()`** | **Seaborn** |
|---|---|---|---|
| **What it is** | the foundation — full control of every element | a thin convenience layer over matplotlib | statistical charts over matplotlib |
| **Best for** | final, publication-quality figures | fast looks while you are still exploring | distributions, categories, small multiples |
| **Input** | lists, arrays, Series | DataFrame / Series (its own data) | DataFrame + column *names* |
| **Effort** | verbose, explicit | one line | short, with statistics included |
| **Defaults** | plain | plain | polished |
| **In this lesson** | Part 2 | the hook, and quick drills | Part 3 |

They are not rivals: seaborn and pandas both return matplotlib objects, so you finish every chart
with `ax.set_title(...)` regardless of which one drew it.
