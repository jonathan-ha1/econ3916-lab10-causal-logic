# Correlation & Causality: Confounders, Colliders and the Limits of Controls

## Objective
Show that the same correlation can come from very different causal structures, and that adding controls can either fix a biased estimate or create a new bias, depending on what is controlled for.

## Methodology
- Simulated three worlds (true causation, pure confounding and pure selection) built so that their correlations all come out near 0.65, even though the true effects are 2.0, 0.0 and 0.0.
- Drew a confounder, a collider and a mediator as directed graphs using networkx to make the causal paths in each structure easy to see.
- Compared regressions with and without controls to show that controlling for a confounder recovers the true effect, while controlling for a collider creates a large, highly significant association between two variables that are actually independent.
- Built an M-bias structure where the control variable is measured *before* treatment, to show that "pre-treatment" does not automatically mean "safe to control for."
- Wrote a small function, `coef_on_d()`, that returns the coefficient on the treatment for any list of controls, and used it to show that also controlling for one of the hidden causes (u1) closes the biasing path again.
- Reproduced the LaLonde (1986) contrast: compared the randomized experimental estimate with an estimate using a comparison group drawn from the CPS, then measured how much of the gap covariate adjustment closes.

## Key Findings
- **Same correlation, different causes:** The three worlds gave correlations of 0.654, 0.664 and 0.632, even though only the first has a real effect (2.0). Correlation alone could not tell them apart.
- **Confounder control:** With a true effect of 1.5, the naive regression gave 2.476. Controlling for the confounder U brought it to 1.500, matching the truth.
- **Collider bias:** D and Y were independent by construction, and the regression without controls gave -0.017 (p = 0.090). Controlling for the collider C gave -0.592 (p = 0.000), a large, highly significant effect where the truth is 0.
- **M-bias:** Without controls the estimate was 0.009. Controlling for the pre-treatment variable W gave -0.191, where the truth is 0. Also controlling for u1 brought it back to 0.006, essentially the truth.
- **LaLonde:** The experimental benchmark was $1,794. The naive CPS comparison gave $-8,498, and adding controls moved it to $699, closing 89% of the gap.
- **Takeaway:** Whether a control helps depends on where it sits in the causal graph. Controls are not a substitute for knowing the causal structure, and even after adjustment the observational estimate ($699) still fell well short of the experimental one ($1,794).
