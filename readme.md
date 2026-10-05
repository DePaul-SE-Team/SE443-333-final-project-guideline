# Final Project: Improving DeepFlash Test Generation with mini-SWE-agent and SWT-Bench

## 1. Objective

Build and evaluate a new workflow that uses DeepFlash to generate regression tests from real-world software issue descriptions. Use mini-SWE-agent as the agent framework for the implementation and evaluate test quality using SWT-Bench.

DeepFlash is the required baseline. Your goal is to add a new workflow intended to improve its bug reproduction success, test quality, or efficiency. Compare the unchanged baseline with your workflow using SWT-Bench and investigate what the results reveal about generated test quality. An improvement is a hypothesis to test, not a required outcome.

### Central Research Question

> Can a new workflow built around DeepFlash generate tests that reproduce reported bugs more effectively or efficiently than the unchanged DeepFlash baseline?

Your grade depends primarily on:

- Experimental rigor
- Reproducibility
- Analysis quality
- Clear reporting

Leaderboard position is reported but is not a major grading factor.

---

## 2. Background

Before beginning:

1. Complete the SWT-Bench paper review.
2. Read the benchmark documentation:
   - https://github.com/logic-star-ai/SWT-Bench
3. Read the mini-SWE-agent documentation: https://github.com/SWE-agent/mini-swe-agent
4. Familiarize yourself with the evaluation workflow.

SWT-Bench evaluates whether generated tests:

- Expose a bug before a fix
- Pass after the fix
- Integrate into the repository test suite

By the end of this project, you should be able to:

- Run a repository-level test generation benchmark
- Design a controlled experiment
- Evaluate AI-generated tests
- Analyze benchmark results critically
- Produce reproducible research artifacts

---

## 3. Project Scope

You may work:

- Individually
- In teams of 2–3 students

Each team must implement and evaluate:

### Required Baseline: DeepFlash

Use [mini-SWE-agent](https://github.com/SWE-agent/mini-swe-agent) as the agent framework and DeepFlash as the baseline model. The mini-SWE-agent is not the evaluation dataset, the assigned SWT-Bench instances remain the shared dataset. Document how your new workflow changes the baseline agent procedure and configure the agent to produce test patches. Generation may not modify production code.

### Experimental Method: A New Workflow Around DeepFlash

Implement one deliberate workflow change intended to improve the DeepFlash baseline. Keep DeepFlash fixed for the primary comparison so that the experiment measures the effect of the workflow.

Choose one primary research question and state why the selected workflow should help.

### Example Workflow: Execution-Guided Test Refinement

One possible experiment is to add a bounded execution-feedback loop:

1. Read the issue and retrieve source code and existing tests.
2. Use DeepFlash to generate a test patch.
3. Apply and run the test on the buggy repository.
4. Classify the result: invalid patch, setup or import failure, unrelated failure, passing test, or potentially issue-relevant failure.
5. Use DeepFlash to revise the test within a fixed repair budget, preserving meaningful assertions.
7. Submit the final patch to the evaluator, which checks behavior before and after the reference fix.

A failing test on the buggy repository is only a candidate reproduction. Generation and repair must not use the reference fix, reference tests, or evaluator-only results. The evaluator determines whether the test satisfies SWT-Bench's success criterion.

This is an example, not a required implementation. A carefully evaluated small improvement is preferable to a complicated system.

---
## 4. Shared Dataset

The instructor will provide:

- Development set (~25 instances) for evaluation
- Dataset revision
- Instance IDs

All teams must evaluate:

- Both methods
- On the same evaluation instances

## 5. Experimental Rules

For a fair comparison:

- Use the same DeepFlash model or implementation and version for both methods. The workflow is the primary research outcome.

Your method may access:

- Issue descriptions
- Repository source code
- Existing tests
- Development tools

Your method may **not** access:

- Reference fixes
- Reference test patches
- Evaluator-only artifacts

Additional requirements:

- Do not modify production code.
- Do not disable tests.
- Do not weaken assertions.
- Preserve all generated outputs.
- Report failures and timeouts.
- Keep missing predictions in the denominator.

---

## 6. Evaluation

Use the official SWT-Bench evaluation harness in **unit-test mode**. 

Report the following metrics below:

### Issue Reproduction Success Rate

Percentage of instances where generated tests:

- Fail before the reference fix
- Pass afterward
- Satisfy the harness success criterion

Report the number of successful instances, the total assigned instances, and the resulting percentage. Missing predictions, generation failures, invalid patches, and method-caused timeouts remain in the denominator and count as unsuccessful.

Log infrastructure failures separately. Retry an unchanged prediction after infrastructure recovery using the same evaluation settings. Do not regenerate predictions or selectively remove instances. Unresolved infrastructure failures remain in the denominator for the class score and must be labeled as unevaluated; explain their effect on interpretation. Any instructor-approved exclusion must apply consistently to every team's methods and be documented.

### Coverage Delta

Report the official harness coverage-delta metric and identify the exact output field, units, aggregation rule, and number of instances with available coverage. Distinguish overall coverage delta from coverage on successful reproductions. Report unavailable coverage as N/A, not zero, and explain why it is unavailable.

### Patch Applicability

Number of assigned instances with a successfully applied prediction patch divided by all assigned instances. Report the count and percentage; missing predictions count as not applicable.

### Cost and Runtime

Report:

- Total generation cost
- Average generation time
- Evaluation time

---

### Paired Comparison and Uncertainty

Provide a table counting instances where:

- Both methods succeed
- Only the baseline succeeds
- Only the experimental method succeeds
- Neither method succeeds

Repeat generation with multiple seeds when the provided budget permits. Otherwise, identify single-run variability as a limitation and record any seeds supported by the tools.

## 7. Class Leaderboard

Submit one row for each method.

| Team | Method | Success Rate | Coverage | Applicability | Cost | Avg. Generation Time |
|--------|--------|--------|--------|--------|--------|--------|
| Example Team | DeepFlash baseline | ... | ... | ... | ... | ... |
| Example Team | DeepFlash + new workflow | ... | ... | ... | ... | ... |

Leaderboard ranking is based on:

1. Success rate
2. Tied methods share rank

Coverage and cost remain visible for comparison.

You should focus primarily on:

- Experimental design
- Analysis
- Reproducibility

rather than leaderboard position.

---

## 8. Required Analysis

Inspect at least **six instances**.

Include:

- Two successful reproductions
- Two unsuccessful cases
- Two cases where the methods disagree

Analyze **six distinct instances**. Categories may overlap, but an instance counts only once toward the total. If a category has fewer than two available cases, analyze all available cases in that category and select additional failures or other available cases to reach six. If fewer than six evaluation instances are available, analyze all of them and explain the shortfall.

State your case-selection procedure. Use a systematic rule, such as selecting by instance ID within each category, and identify any additional cases chosen for a specific diagnostic reason.

For each instance discuss:

1. The reported issue
2. The generated test
3. Behavior before the fix
4. Behavior after the fix
5. Whether the test meaningfully represents the issue
6. What the result reveals about the method

Also discuss:

- At least one threat to validity
- At least one future improvement

Examples:

- Weak assertions
- Flaky tests
- Benchmark contamination
- Limited repository coverage
- Overfitting to benchmark behavior

---
## 9. Milestones

| Milestone | Suggested Timing | Deliverable |
|------------|------------|------------|
| Proposal & Pilot | Week 6 | Research question, baseline, planned improvement, budget |
| Development Checkpoint | Week 7 | Working methods and preliminary results |
| Method Freeze | Week 8 | Frozen configuration and evaluation procedure |
| Final Submission | Week 9 | Report, artifacts |
| Presentation | Week 10 | Oral Presentation (e.g., ten minutes video or in-person or online) |

### Pilot Requirement

The Week 6 pilot must demonstrate that you can:

- Generate a test patch
- Run the evaluation harness
- Interpret evaluation results

---

## 10. Prerequisites, Resources, and Support

Students should have basic experience with Python, Git, automated testing, and Docker. 

## 11. Final Submission

Submit a repository or ZIP archive containing:

### Report

1500–2000 words for the main text, excluding references and appendices, using the [IEEE conference template](https://www.overleaf.com/latex/templates/ieee-conference-template/grfzhhncsfqn).

Required sections:

1. Research Question and Motivation
2. Method and Experimental Design
3. Results
4. Test Quality Analysis
5. Threats to Validity
6. Conclusion

Include the main findings from the case studies in the report. Detailed six-instance analyses, generated test excerpts, and additional result tables may appear in an appendix.

### Artifacts

- Source code
- README
- Prediction JSONL files
- Evaluation results
- Logs
- Generated patches
- Frozen DeepFlash baseline and experimental workflow configurations, including prompts
- mini-SWE-agent source, version, configuration, and integration instructions
- Workflow diagram or numbered procedure, stopping rules, and final-candidate selection rule
- Per-instance paired outcomes and cost records
- A contribution statement describing each team member's implementation, evaluation, analysis, and writing work

### Presentation

A ten-minute presentation covering:

- Research question
- Method
- Results
- Main insight
- Biggest limitation

---

## 12. AI Disclosure

Disclose all AI tools used for:

- Test generation
- Implementation
- Analysis
- Writing

Identify which outputs were reviewed or modified by your team.

---

## 13. Project Grading (30% of Course Grade)
 
The final project contributes **30% of the overall course grade**.
 
| Category | Points |
|-----------|--------:|
| Research question and experimental design | 15 |
| Baseline and experimental method | 20 |
| Correct evaluation and reporting | 20 |
| Reproducibility and artifacts | 20 |
| Analysis and discussion | 15 |
| Report and presentation | 10 |
| **Total** | **100** |

### Important

A method that performs worse than the baseline can still earn full credit if:

- The experiment is well designed
- The evaluation is fair
- The results are reproducible
- The analysis is thoughtful and insightful

---

## Reference

Mündler, N., Müller, M. N., He, J., & Vechev, M. (2024).

**SWT-Bench: Testing and Validating Real-World Bug-Fixes with Code Agents.**

Advances in Neural Information Processing Systems (NeurIPS 2024).

https://doi.org/10.48550/arXiv.2406.12952


