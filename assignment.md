# 📝 Assignment: Visualisation & Storytelling — The Board Pack

> ⏱️ **Estimated Time:** 80–95 minutes | Complete this **after** your class session.

---

## 🎯 Learning Objectives Revisited

This assignment reinforces what you practised in class:

- Applying the three pillars to critique and redesign a chart
- Building charts with the Figure → Axes → plot hierarchy
- Choosing the statistical view the data type calls for
- Selecting a chart — and a depth of detail — to fit the message and the audience

**The one rule applies to every task:** *the chart is chosen by the message, not by taste.* Write the
sentence first. If you cannot write it, you do not yet know what the chart is for.

---

## Part 1: Conceptual Check (15 min)

**Question 1:** A colleague shows you a bar chart of four outlets' revenue with the y-axis starting at
150,000. He says "it makes the difference clearer". What is your response — and is there *any* version
of this chart that would be acceptable?

**Question 2:** You have a line chart of monthly revenue, and the y-axis starts at 25,000 rather than 0.
Is this the same offence as Question 1? Explain in terms of what each chart type encodes.

**Question 3:** Rewrite these three titles so each states a finding: "Monthly Revenue", "Revenue by
Outlet", "Ticket Value Distribution". Invent plausible findings from the café data you know.

**Question 4:** You have to compare twelve outlets over eighteen months. Sketch (in words) two options
and say which you would choose for a manager, and why.

**Question 5:** The mean ticket is \$8.38 and the histogram shows two humps — one around \$6, one around
\$15. Your manager asks for "the average customer" for a promotion. What do you say?

<details>
<summary>💡 Check Your Answers</summary>

**Q1:** A bar encodes value as **length**, so a non-zero baseline makes the length disproportionate to
the value — the reader's eye does arithmetic on a false premise. In class, the truncated version made
Marina Bay look like it earned a tenth of what Raffles Place earns; it earns 66% of it. And yes, there is an acceptable
version: if the *differences* are the story rather than the magnitudes, use a chart type that encodes
position instead of length — a dot plot, or a bar chart of the *change* rather than the level (which is
zero-based in its own terms). What is not acceptable is keeping the bars and quietly moving the baseline.

**Q2:** No, it is not the same offence. A line encodes **position over time** and is read for *change*,
not magnitude. Forcing a zero baseline often flattens a real, decision-relevant movement into a straight
line. The honest requirement for a line chart is different: label the axis clearly so the reader knows
the scale does not start at zero. Rule of thumb — **bars start at zero; lines start wherever the change
is legible.**

**Q3:** For example: "Chain revenue has been flat for two quarters"; "Marina Bay has fallen 28% while
Holland Village has grown 27%"; "Most customers buy a \$6 drink; a minority buy a \$15 meal". Notice you
cannot write any of these without knowing your finding — which is exactly why title-writing is a useful
discipline rather than a formatting step.

**Q4:** Option A: twelve lines on one set of axes — a spaghetti chart, unreadable, no outlet followable.
Option B: twelve small panels (small multiples) on a **shared y-axis**, so the shapes are comparable.
For a manager, choose B, and go further: grey eleven panels and colour the one that needs action. A
manager needs to know where to intervene, not to admire twelve trends.

**Q5:** There is no average customer — the mean lands in the *dip between* two groups and describes
almost nobody. Say so, then offer the useful version: two segments, one promotion each (a drinks offer
and a meal offer), or pick the segment that matters for the goal. If the goal is footfall, target the
\$6 group; if it is basket size, target moving \$6 customers into the \$15 group. Reporting "the average
customer spends \$8.38" would send the promotion at a person who does not exist.

</details>

---

## Part 2: Practical Challenges (60–75 min)

### Scenario: "The Board Pack"

The owner found your analysis useful and is now taking it to her two investors. She has asked for a
short pack: three charts, each answering one question, each able to stand on its own.

Start a new notebook in `notebooks/` and set up:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

sales = pd.read_csv("../data/daily_sales.csv", parse_dates=["date"])
outlets = pd.read_csv("../data/outlets.csv")
tickets = pd.read_csv("../data/tickets_week.csv", parse_dates=["txn_datetime"])
decision = pd.read_csv("../data/lesson19_decision.csv", index_col="outlet_id")

names = outlets.set_index("outlet_id")["outlet_name"].to_dict()
sns.set_theme(style="whitegrid", palette="colorblind")
```

---

### Challenge 1: The critique and redesign

Below is a chart request as it usually arrives, and a first attempt at it.

```python
# The "before" -- please run this, and do not fix it yet.
weekday = sales.copy()
weekday["day"] = weekday["date"].dt.day_name()
weekday.pivot_table(index="day", columns="outlet_id", values="revenue_sgd", aggfunc="sum").plot(kind="bar")
plt.show()
```

**Tasks:**
1. Write down **five** things wrong with this chart. For each, name the pillar it violates
   (Perception / Design / Storytelling).
2. Decide what the chart is actually *for*. Write the one-sentence message.
3. Redesign it. You may change the numbers (a different aggregation is fair game), the chart type, the
   colours, the labels and the title — anything that serves your sentence.
4. In two sentences, say what your redesign gives up. Every redesign trades something away.

<details>
<summary>💡 Hint</summary>

The days come out in alphabetical order, which is meaningless. The totals mix outlets of different
sizes. Codes instead of names. No title, no units. And ask the important question: is *total* revenue
per weekday even the right number, when there are 79 Mondays but a different number of trading days
elsewhere? An average per day is probably what anyone means.

</details>

<details>
<summary>✅ Solution</summary>

**Five problems:**

| Problem | Pillar |
|---|---|
| Days are in alphabetical order (Friday, Monday, Saturday…) | Design — the axis has an obvious natural order and it is being ignored |
| Sums, not averages per day — distorted by unequal day counts | Design — wrong aggregation for the question |
| Five colours, all equally loud, one of them a closed kiosk | Perception — the attention budget is spent on nothing |
| Outlet codes rather than names | Perception — forces a translation step |
| No title, no units, no message | Storytelling — the reader has to guess the point |

**The message:** *"Raffles Place and Marina Bay are commuter cafés — they empty at the weekend.
Tampines Mall is the opposite."*

```python
# One row per outlet per day, so we can average by weekday honestly.
daily = sales.groupby(["date", "outlet_id"], as_index=False)["revenue_sgd"].sum()
daily["day"] = daily["date"].dt.day_name()

order = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]
keep = ["OUT-01", "OUT-02", "OUT-03", "OUT-04"]

avg = (
    daily[daily["outlet_id"].isin(keep)]
    .pivot_table(index="day", columns="outlet_id", values="revenue_sgd", aggfunc="mean")
    .reindex(order)
)

# Index each outlet to its own weekday average = 100, so shapes are comparable across sizes.
indexed = avg.div(avg.mean(axis=0), axis=1) * 100

fig, ax = plt.subplots(figsize=(9, 4))

# The message is a TWO-GROUP comparison, so it needs two colours -- one per group, not one
# per outlet. Same line style within a group; the reader learns "red = CBD" once and is done.
styles = {
    "OUT-01": ("#c0392b", "-", "Raffles Place (CBD)"),
    "OUT-03": ("#c0392b", "--", "Marina Bay (CBD)"),
    "OUT-02": ("#7f8c8d", "-", "Tampines Mall (mall)"),
    "OUT-04": ("#7f8c8d", "--", "Holland Village"),
}
for outlet, (colour, dash, label) in styles.items():
    ax.plot(indexed.index, indexed[outlet], color=colour, linestyle=dash, linewidth=2.2,
            marker="o", markersize=4, label=label)

ax.axhline(100, color="#95a5a6", linewidth=0.8, linestyle=":")
ax.set_title("The two CBD cafés lose half their trade at the weekend",
             loc="left", fontsize=13, weight="bold")
ax.set_ylabel("revenue vs the outlet's own weekly average (100 = average day)")
ax.legend(frameon=False, fontsize=9)
ax.spines[["top", "right"]].set_visible(False)
plt.tight_layout()
plt.show()
```

**On the two colours:** Part 1 said "one accent colour, the rest grey", and this chart uses two. That
is not a contradiction — the message here is a *two-group* comparison, so the colour is doing the
grouping work, and the dashed/solid distinction separates outlets *within* a group without spending
another colour. The rule is really "one visual distinction per idea in your message". One idea, one
colour; two groups, two colours; four unrelated colours, no idea.

**What the redesign gives up:** the absolute dollars are gone, so this chart cannot answer "how much
does Raffles Place make on a Monday?" — indexing to 100 buys comparability at the cost of magnitude.
It also drops the pop-up kiosk entirely. Both are the right trade *for this message*, and both would be
wrong if the question had been "where does our money come from?". State the trade-off; do not pretend
there isn't one.

</details>

---

### Challenge 2: The distribution chart

The investors ask: *"is Marina Bay's problem that customers spend less, or that there are fewer of them?"*

**Tasks:**
1. Build **one** chart from `tickets` that answers this directly.
2. Add a second chart that shows the other half of the answer (ticket *counts* rather than values).
3. Title each with its finding.
4. One sentence: which chart would you show first, and why does the order matter?

<details>
<summary>💡 Hint</summary>

Ticket *values* come from `tickets_week.csv`; ticket *counts* per day come from the `tickets` column in
`daily_sales.csv`. Careful with names — you have a DataFrame called `tickets` and a column called
`tickets`. This is a real-world annoyance worth noticing, not a trick.

</details>

<details>
<summary>✅ Solution</summary>

```python
fig, axes = plt.subplots(1, 2, figsize=(12, 4))

# ---- left: do they spend less per visit? (no) --------------------------------
keep = ["OUT-01", "OUT-02", "OUT-03", "OUT-04"]

sns.boxplot(data=tickets[tickets["outlet_id"].isin(keep)], x="outlet_id", y="amount_sgd",
            order=keep, color="#cfd4d8", ax=axes[0])

# Recolour just the Marina Bay box. Doing it this way (rather than with `palette=`) works on
# every seaborn version -- the boxes are ordinary matplotlib patches, in the order you asked for.
axes[0].patches[keep.index("OUT-03")].set_facecolor("#c0392b")
axes[0].set_xticklabels([names[o] for o in keep], rotation=15)
axes[0].set_title("Not spending less: Marina Bay's tickets match Raffles Place's",
                  loc="left", fontsize=11, weight="bold")
axes[0].set_xlabel("")
axes[0].set_ylabel("ticket value (S$)")

# ---- right: are there fewer of them? (yes) -----------------------------------
counts = (
    sales[sales["outlet_id"] == "OUT-03"]
    .groupby("date")["tickets"].sum()
    .resample("M").mean()
)

axes[1].plot(counts.index, counts.values, color="#c0392b", linewidth=2.4)
axes[1].axvline(pd.Timestamp("2024-11-04"), color="#95a5a6", linestyle="--", linewidth=1)
axes[1].set_ylim(0, counts.max() * 1.2)
axes[1].set_title("Fewer of them: Marina Bay's daily customers, monthly average",
                  loc="left", fontsize=11, weight="bold")
axes[1].set_ylabel("tickets per day")
axes[1].spines[["top", "right"]].set_visible(False)

plt.tight_layout()
plt.show()
```

**Read the left panel carefully — and title it carefully.** All four medians sit near \$6.40, but the
two mall/neighbourhood outlets have taller boxes: they sell more meals, so more of their tickets are
large. The honest claim is the comparison that matters — **Marina Bay's tickets are indistinguishable
from Raffles Place's**, its nearest twin — not the broader "all four are identical", which the chart
does not quite support. Titles are claims, and a chart should support the claim you put on it.

**Which first, and why the order matters:** show the box plot first. It *eliminates* the explanation
everyone reaches for — "our prices or our customers must have changed" — and an audience that has
discarded its own first theory is ready to accept yours. Lead with the count chart and half the room is
still privately wondering about pricing while you talk.

Note also the y-limit on the right: it is a *count* per day, and starting the axis at zero here keeps
the size of the drop honest even though it is a line chart.

</details>

---

### Challenge 3: Your own one slide

Build a single figure for the investors. The message is yours to choose, but it must be defensible from
the data, and the figure must work with **no voiceover**.

**Requirements:**
1. One or two panels, no more.
2. The message as the figure title — a sentence, not a metric name.
3. One accent colour; everything else grey.
4. At least one annotation naming a cause, an event, or a threshold.
5. Bars from zero. Axis labels with units.
6. Saved with `fig.savefig("../visualisations/my_board_slide.png", dpi=150, bbox_inches="tight")`.
7. Below the cell, write your three-act script — one sentence per act.

<details>
<summary>💡 Hint</summary>

The strongest messages available in this data are: the cancelling trends; the November step; the
rent-share gap; the staffing efficiency gap; and the commuter-versus-neighbourhood split. Pick **one**.
A slide with two messages has none.

</details>

<details>
<summary>✅ One possible answer</summary>

There is no single right answer here — this one takes the staffing angle, deliberately *not* the angle
used in class, to show that the same data supports more than one honest slide.

```python
weekly = (
    sales.groupby(["outlet_id", pd.Grouper(key="date", freq="W-MON", label="left")])["revenue_sgd"]
    .sum().reset_index().rename(columns={"date": "week_start"})
)
roster = pd.read_csv("../data/roster.csv", parse_dates=["week_start"])
staffed = weekly.merge(roster, on=["outlet_id", "week_start"], how="inner")
staffed["per_hour"] = staffed["revenue_sgd"] / staffed["staff_hours"]

fig, ax = plt.subplots(figsize=(9.5, 4.2))

for outlet in ["OUT-01", "OUT-02", "OUT-04", "OUT-03"]:
    d = staffed[staffed["outlet_id"] == outlet].sort_values("week_start")
    smooth = d.set_index("week_start")["per_hour"].rolling(8).mean()
    accent = outlet == "OUT-03"
    ax.plot(smooth.index, smooth.values,
            color="#c0392b" if accent else "#cfd4d8",
            linewidth=2.8 if accent else 1.5,
            zorder=3 if accent else 1,
            label=names[outlet] if accent else None)

ax.axvline(pd.Timestamp("2024-11-04"), color="#95a5a6", linestyle="--", linewidth=1)
ax.annotate("revenue fell here.\nthe rota never did.",
            xy=(pd.Timestamp("2024-11-04"), 24),
            xytext=(pd.Timestamp("2024-12-15"), 20),
            fontsize=10, color="#c0392b",
            arrowprops=dict(arrowstyle="->", color="#c0392b", linewidth=1))

ax.set_ylim(0, 32)
ax.set_ylabel("revenue per staff hour (S$), 8-week average")
ax.set_title("Marina Bay is now paying for staff its customers stopped needing",
             loc="left", fontsize=13, weight="bold")
ax.legend(frameon=False, loc="lower left")
ax.spines[["top", "right"]].set_visible(False)
plt.tight_layout()
fig.savefig("../visualisations/my_board_slide.png", dpi=150, bbox_inches="tight")
plt.show()
```

**The three-act script:**

- **Act 1 (context):** "All four cafés earned about the same per staff hour until last November."
- **Act 2 (finding):** "Marina Bay's revenue stepped down 21% in one week when a competitor opened, and
  the roster was never re-cut — so it now earns \$20 per staff hour against \$25–27 everywhere else."
- **Act 3 (decision):** "Re-sizing that rota is a decision we can make this month, independently of the
  lease — and we should make it before we decide the lease, so we know what the outlet is really worth."

Notice the accent colour is spent on exactly one line, the annotation names a cause, and the y-axis
starts at zero because "per hour" magnitudes are being compared. Act 3 offers a decision the audience
can actually take.

</details>

---

### 🏆 Stretch Challenge (optional): the honest misleading chart

Build **two** charts from the same data that support **opposite** conclusions — one arguing Marina Bay
should be kept, one arguing it should be closed — without fabricating a single number.

Then write a paragraph on what you had to do to each one, and what that tells you about charts you are
shown by other people.

<details>
<summary>💡 What to notice</summary>

The techniques available without touching the data: choose the time window (Marina Bay looks fine if
you start in 2023 or only show pre-November data); choose the metric (total revenue versus revenue per
staff hour versus rent share); choose the comparison (against the chain average, against its own past,
against the kiosk); truncate or zero the axis; aggregate to quarters to smooth the step away; include
or exclude the pop-up kiosk from the chain total.

**The lesson is not "charts lie".** It is that every chart embeds a set of choices — window, metric,
comparison, aggregation, scale — and the honest analyst makes those choices explicit rather than
invisible. When you are *shown* a chart, the useful question is not "is this number right?" but "what
was chosen, and what would the other choice have shown?"

</details>

---

## 💬 Reflection (5 min)

In 2–3 sentences:

> Think of a chart you have made or been shown at work. Applying what you now know about the three
> pillars, what was the one change that would have improved it most — and why *that* one?

---

## 📤 Share Your Work

Post your Challenge 3 slide (the PNG) and your three-act script in the **#peer-reviews** Discord
channel. Critique two others using the pillar language: name the pillar, name the fix. For questions,
post in **#questions**.
