# Project Retrospective — GSK x DigData Step Up Career Challenge

*Following Gibbs' Reflective Cycle*

## 1. Description

Over the course of this challenge I worked on **both** Path A (clinical trial data analysis) and Path B (web tool design) for the Miraculon-B oncology dataset. Path A involved cleaning a 772-row clinical study, merging it with a separate protein-levels file, performing aggregation and visualisation, fitting a logistic regression and a random-forest model, and presenting the findings to a non-technical stakeholder audience. Path B used a clean 768-row dataset that also included an adverse-event flag, and the deliverable was a working HTML/CSS/JS prototype of a prescriber decision-support web app, plus a design-thinking presentation explaining it.

## 2. Feelings

Starting out, the prospect of doing both paths felt large but exciting — the dataset is small enough to grasp end-to-end, but the questions are real ones an industry team would actually ask. Once the headline finding emerged from the aggregation step (response rate jumping from 32.6% in control to 55.2% on Miraculon-B), I felt energised. The protein-biomarker pattern was satisfying to uncover because it unified both paths: the same variable that drives response in Path A also drives the prescriber's dilemma in Path B. The build phase of the prototype was the most enjoyable — turning numbers into a tool a doctor could actually use. The hardest emotional moment was deciding what to leave out: there is always more analysis to do, and the temptation to keep adding charts had to be balanced against clarity for a non-technical audience.

## 3. Evaluation

**What went well.** The data wrangling pipeline was clean and reproducible: deduplication → paediatric exclusion → NA handling → BMI engineering → merge on participant ID. The headline result was statistically robust (χ² = 38.0, p ≈ 7 × 10⁻¹⁰) and the modelling stage gave a satisfying AUC of 0.86, with protein concentration emerging as by far the dominant predictor. The Path B prototype works live — every slider movement updates the verdict banner, predicted response %, AE %, and net-benefit comparison. The two decks share a visual language (GSK orange, dark slate, sand neutral, serif headlines) which makes them feel like one body of work.

**What did not go as well.** The protein binning in the prototype's calculator is a look-up table, not a real fitted model — it is honest about that on the "Built today / Next iterations" slide, but a calibrated logistic regression would be more correct. The visualisations could have included a bivariate view of protein × treatment to show the interaction directly, rather than relying on the bucketed table alone. I also did not validate the prototype with anyone resembling a clinician — for a tool that is meant to support prescribing decisions, even a single user-test session would have improved the design.

## 4. Analysis

The dominant lesson is that **a single biomarker can change the entire risk-to-benefit calculus** of a drug, and that effect is invisible in the headline trial summary. Miraculon-B's marginal effect (+22 percentage points response on average) is real, but the per-patient effect ranges from "essentially curative" at low protein concentrations to "actively harmful" at high ones. This is exactly the situation where a static drug label fails and a decision-support tool can add value.

Methodologically, what helped most was **doing the analysis before designing the UI**. The protein bucket structure (<80, 80–100, 100–120, 120–140, >140 ng/mL) emerged from the data and then naturally became the structure of the calculator's logic. What hindered me was occasionally over-investing in chart polish before the underlying calculation was stable — it is faster to iterate on a rough chart than to redo a polished one.

## 5. Conclusion

If I were doing this again, I would (a) start with a single shared "data spine" — one cleaned, merged dataset used by both the analysis and the prototype, rather than treating them as parallel streams, and (b) build a paper wireframe of the calculator before writing any HTML, to separate the design decisions from the implementation decisions. I would also be more disciplined about citing **uncertainty** alongside every prediction in the calculator — confidence intervals on response and AE rates, not point estimates.

## 6. Action plan

For a similar future project I will:

- Spend the first hour on a **decision-support storyboard** — what does the user see, in what order, leading to what action — before touching any data.
- Build a **shared `prep.py` / `prep.R` script** that produces one canonical merged dataset, used everywhere downstream.
- Validate the prototype with **at least one domain user** (a medical student or pharmacist would do) before declaring it finished.
- Replace the bucketed look-up with a **real fitted model** persisted to JSON, so the same coefficients drive both the analysis report and the live calculator.
- Add **uncertainty intervals** to every predicted number in the UI — point estimates without uncertainty are a known anti-pattern in clinical decision support.

The most important takeaway: a data role is not just analysis or just product — it is the bridge between them. The clinical trial result only matters when it reaches the prescriber in a form they can use in the next ten minutes of their consultation.
