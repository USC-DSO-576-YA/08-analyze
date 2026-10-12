# Module 8 — Analyze and solve with pandas

## Class continuation: dashboard prompting

The [dashboard prompting activity](dashboard_prompting.md) continues the Module
7 wildfire dashboard work. The verified monthly data is bundled in
`data/monthly_wildfire.csv`. [plan.md](plan.md) shows the thinking behind the
prompt; [BUILD_SPEC.md](BUILD_SPEC.md) is the runnable specification for a
polished, self-contained HTML dashboard. Use Codex to build it and check the
result. **This activity is separate from the homework.**

## Four-problem homework

The **homework** is
`module08_creative_problem_solving_homework.ipynb`. It has **four problems in
total**. Complete all four in the notebook. Each gives a business objective,
operating rules, and a requested report. Write the expected output from the
sample data before deciding which intermediate pandas calculations and tables
you need.

The problems cover an inspection shortlist, a repeating cash cycle, hospital
ward transfer capacity, and delivery-van routes with planned fuel stops.

Problem 3 includes a brief data exchange with a partner: each of you receives
one ward's monthly staffed-bed counts from the instructor, and you share the
records before coding.
Each student writes their **own** plan, function, and result explanation. The other three
problems are individual work.

1. In this folder, run `uv sync --frozen` and select `.venv` as the notebook
   kernel in VS Code.
2. Read each business request, write its expected output and your plan before
   coding, then write the function, display the result, and explain what it
   shows.
3. Save and submit this one notebook on Gradescope. Keep
   your work local; do not push to the shared course repository.

Gradescope awards one point per problem for a code attempt, regardless of
correctness. It also displays pass/fail checks with each test's input, expected
output, and your output or error. Those checks are feedback and do not change
the attempt score.

**Codex may build and debug the wildfire HTML dashboard and your team project
dashboard. Do the four-problem homework coding yourself.** The repository's
`AGENTS.md` gives Codex that boundary.
For extra practice, ask Codex to read `tutor.md`; it will invent different
problems of comparable difficulty and give feedback on those practice attempts.
`agent.md` explains how it should guide your reasoning. The automatic
`AGENTS.md` policy keeps this tutoring separate from the assigned homework.

The supplied tables are small, invented examples. Use pandas transformations
without explicit row-by-row Python loops. The challenge is to decide which
calculations are needed to reach the requested business result.
