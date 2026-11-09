# Abigail Ray || ECON 5200 || Causal Logic and DAGs: Diagnosing Bad Controls

## Objective

I tested when adding a control variable fixes a causal estimate and when it breaks one, using simulations where I knew the true effect and a real job-training experiment where the right answer can be checked.

## Methodology

- Simulated a treatment with no true effect and added a control caused by both the treatment and the outcome, then compared the estimate with and without that control.
- Simulated a control measured before treatment that was still caused by two hidden factors, one linked to treatment and one to the outcome, to test the rule that pre-treatment controls are always safe.
- Simulated a training programme that raises earnings partly by raising skill, and estimated the effect with and without controlling for skill to separate the total effect from the direct effect.
- Used the LaLonde (1986) job-training data from Dehejia and Wahba: computed the experimental benchmark, then compared it with an observational estimate using men from the CPS as the comparison group, before and after adjusting for age, education, race, marital status, degree and 1974–75 earnings.
- Used a `dag_utils.py` module that lists the backdoor paths in a causal graph and checks whether a set of controls blocks all of them.
- Asked an AI which controls to use for a job-training study, checked its answer against the study's causal graph with `dag_utils.py`, revised my prompt once, and checked the new answer.

## Key Findings

- **A control caused by both treatment and outcome creates an effect that isn't there.** The true effect was zero and the simple regression found -0.012. Adding the control moved the estimate to -0.593 and raised R-squared to 0.585. The better fit and smaller p-value came from the control containing part of the outcome, not from a better estimate.
- **Measuring a control before treatment does not make it safe.** A pre-treatment control fed by two hidden causes produced an estimate of -0.201 against a true effect of zero. Whether a control is safe depends on what causes it, not on when it was recorded.
- **Controlling for a mediator answers a different question.** Without controlling for skill, I estimated the total effect of training at 3.978 (truth 4.0). Controlling for skill gave the direct effect, 0.988 (truth 1.0). Neither number is wrong. To decide whether to fund the programme, the total effect is the one that matters.
- **Adjustment can move an estimate toward the truth without reaching it.** The LaLonde experiment's benchmark was $1,794. The observational comparison with CPS controls gave -$8,498 before adjustment and $699 after. Without the experiment there would be no way to know that $699 is still well short.
- **The AI's first answer needed checking.** Its first set of controls included a caseworker risk score because it was recorded before training, and it gave 3.622 against a true effect of 4.0. The risk score is computed from two hidden factors, one that drives enrolment and one that drives earnings, so controlling for it opened a new biased path. After I revised my prompt to tell the AI not to judge controls by timing alone, its set gave 4.047.
