# Class continuation: prompt Codex for a wildfire HTML dashboard

This activity continues the **Module 7 wildfire dashboard** work. It is **class work, not part of the four-problem homework**. The monthly table from Module 7 is bundled here as `data/monthly_wildfire.csv`, so the example can run without a student's earlier notebook output. Codex may build and revise this dashboard.

[`plan.md`](plan.md) shows the thinking and structure behind the prompt. [`BUILD_SPEC.md`](BUILD_SPEC.md) is the complete, runnable prompt. The structure resembles the graded Project Phase 2 spec, but your team's `BUILD_SPEC.md` must describe **your own project and data**.

## Before using Codex

Read the wildfire `BUILD_SPEC.md` and find these parts:

1. The user, decision, and **select → see → decide** use cases.
2. The real data columns and the meaning of each displayed number.
3. Components with inputs, outputs, and a correctness check.
4. A build order and acceptance checks that could catch a convincing-looking wrong chart.

Choose one visual or checking detail you would improve. Write a sentence explaining your choice. This is your own prompt judgment; the example spec is a starting point, not a model answer for your team project.

## Run the example

In this Module 8 repository, run `uv sync --frozen`. Ask Codex: **“Implement BUILD_SPEC.md in this repository, including its checks.”** You may add your one-sentence improvement to that request. Codex should create reusable analysis code, a build script, and a self-contained `dashboard.html`. Run the build command it reports and open the HTML file directly in a browser.

## Check it yourself

- The default view shows **8,182** reported records for 2024 and **8,191** for 2023, a difference of **−9**. These values must come from the CSV.
- Select two years and confirm the comparison lines align January with January.
- Change only the comparison year. The focus-year charts should remain the same.
- Check one chart value against a row in `data/monthly_wildfire.csv`; check that January's average can include the previous year's months.
- Confirm the page opens offline, works on a narrow screen, and keeps **reported records** distinct from **known reported acres**.

Be ready to show the prompt you sent, the improvement you chose, one check you performed, and any corrective follow-up prompt. Do not put this dashboard activity in the homework notebook.
