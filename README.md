# PTO & Leave Tracker 2026

A calendar-year leave planner for university staff. It tracks vacation accrual, PTO and floating holiday grants, university holidays, and the winter closure. It then works out **how many hours to use each month so none are lost** to a year-end reset or the vacation cap.

**[Live demo →](https://jstndxn.github.io/pto-leave-tracker/)**


## The problem

Staff juggle three leave types with different rules, plus a two-week winter closure:

| Leave type | How it's earned | Rule |
|---|---|---|
| Vacation | 0.07693 hrs per hour on straight-time pay status (about 13.33 hrs/month, or 20 days a year at 40 hrs/week) | Accrual stops at **240 hrs** |
| PTO | 24 hrs granted Jan 1 | Use it or lose it on Dec 31 |
| Floating holiday | 16 hrs granted Jan 1 | Use it or lose it on Dec 31 |

The **winter closure** (Mon Dec 21, 2026 – Fri Jan 1, 2027) has 10 workdays. Holidays and additional paid days are covered, but every other closure day needs leave hours. People who use up PTO earlier in the year often find they don't have enough left for the closure.

## Features

- **Locked winter closure hours.** Each closure day is marked as a holiday, an additional paid day, or a leave day. Leave days are covered automatically, using use-it-or-lose-it hours first, and those hours are set aside so the monthly suggestions never spend them. There's also a custom split option.
- **Vacation cap planner.** For each month at the 240-hr cap, it shows the minimum hours to use and places them on open weekdays in a calendar. It gives exact times off, such as "leave at 11:09 AM".
  - **Rounding:** exact (to 0.01 hr), or rounded up to 15 min, 30 min or 1 hr
  - **Plan styles:** one full day plus one extra day; the same amount split evenly over a set number of days (default 3); one shorter day each week; or a little every day
  - Add the plan to your log with one click
- **Work schedule.** The default is 8:30 AM – 5:00 PM with a 30-minute unpaid lunch, which is 8 paid hrs/day and 40 hrs/week. You can pick which weekdays you work, and the schedule drives accrual, leave-day math and the time-off ranges.
- **"Use this month" plan** for each leave type, plus a combined total in hours and days.
- **Time off log** with date ranges. Weekends and university holidays are skipped automatically, so a Thanksgiving-week entry only counts Mon–Wed.
- **Current balances** calculated from Jan 1 balances, accrual, and logged time.
- **Cap forecast** shows when vacation will hit 240 hrs and how much to use to stay under it.
- **At-risk warnings** for hours that would be lost by Dec 31.
- **Editable holiday list** for 2026, with Saturday holidays observed on Friday.
- **Month-by-month chart and table** for each leave type.
- **Export/Import** your data as JSON. Everything runs in the browser.

## How the calculations work

**Vacation accrual**
```
monthly accrual = 0.07693 × hours paid per week × 52 ÷ 12
                = 0.07693 × 40 × 52 ÷ 12 ≈ 13.33 hrs/month (≈ 160 hrs/year)
```
Hours post at the end of each month, up to the cap.

**Use it or lose it (PTO, floating holiday)**
```
hours to spread = balance + remaining accrual − logged time − locked closure hours
```
The remaining hours are spread over the months left in the year, which evens out total time off each month.

**Vacation cap**
```
minimum to use this month = balance − logged time + this month's accrual − 240
```

**Placing hours on weekdays**
```
paid hrs/day = end − start − unpaid lunch      (8:30–5:00, 30 min lunch → 8 hrs)
partial day  = leave early: work (8 − hours off), skipping lunch if you work past it
```
Example: a month at the cap needs 13.3345 hrs.

| Rounding | Plan style | Result |
|---|---|---|
| Exact | Full day + one extra day | 8 hrs (full day) + 5.34 hrs (leave at 11:09 AM) |
| Exact | Split over 3 days | 4.45 + 4.45 + 4.44 hrs (leave at 12:33 PM each day) |
| 15 min | Full day + one extra day | 8 hrs + 5.5 hrs (leave at 11:00 AM) |

Leave times are rounded down to the minute so time off is never short.

**Winter closure**
```
leave hours needed = (closure workdays − holidays − additional paid days) × 8
```
With 2 holidays and 3 additional paid days, 5 days (40 hrs) need leave. That equals the 24 PTO + 16 floating holiday hours granted on Jan 1. By default, the tracker locks those hours for December, so you can use vacation freely the rest of the year.

## Run it

Open `index.html` in any browser. There's nothing to install or build.

### Publish on GitHub Pages
1. Push this folder to a public repo named `pto-leave-tracker`.
2. Go to **Settings → Pages**, set the source to the `main` branch and `/ (root)`, then save.
3. The app goes live at `https://jstndxn.github.io/pto-leave-tracker/`.

## Tech

- Plain HTML, CSS, and JavaScript in one file, with no dependencies
- The calculation engine (`<script id="engine">`) is pure functions, separate from the UI, so it's easy to test
- Responsive layout with automatic light and dark mode

## Data and accuracy

- The leave rules are based on published staff policy. Holiday and closure dates should be checked against the official university holiday schedule, since they can change.
- The vacation starting balance (80 hrs) and the Thanksgiving entry are made up for the demo.
- This is a personal planning tool, not an official system of record.

## Ideas for next steps

- Biweekly pay-period accrual for non-exempt staff
- Part-time (FTE-based) PTO and floating holiday proration
- Calendar export (.ics) for planned time off
- A team view for planning coverage
