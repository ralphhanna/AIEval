# AI-Eval Assessment — sobolevn/python-code-disasters

Repository: `sobolevn/python-code-disasters`

## Scope note
This repository appears to be intentionally curated as a collection of **bad Python code examples / anti-patterns**, not as a production-ready application or library. The evaluation below follows the AI-Eval instruction set **as if the repository itself were being judged as a codebase**, which naturally yields low scores.

## Overall score: 1.9 / 10

## Category scores

### architecture_and_design — 2/10
**Summary**  
The repository is organized into topical folders (`django`, `flask`, `obfuscation`, `python`), which is the main structural strength. Beyond that, there is no clear architectural model, no separation of concerns at repository level, and no design narrative aimed at building a maintainable software system.

**Strengths**
- Very simple top-level categorization by theme/framework.
- Easy to understand the repository’s intent from the README.

**Weaknesses**
- No cohesive system architecture.
- No module boundaries or reusable abstractions visible at repository level.
- No design docs, conventions, or patterns for extensibility.

**Findings**
- The repository explicitly states it contains code “so bad, that it is worth sharing.”
- The maintainer also says “It is still not clear to me, how to structure this project.”

**Recommendations**
- Add a clear taxonomy for samples: e.g. `error-handling/`, `state-management/`, `naming/`, `security/`, `framework-specific/`.
- Add a schema for each example: problem, why it is harmful, safer rewrite, lessons learned.
- Separate “example inputs” from “reference corrected implementations”.

### code_quality_and_readability — 1/10
**Summary**  
Readability and code quality are intentionally poor by project design. That makes the repository useful as a cautionary collection, but it scores extremely low as a software codebase.

**Strengths**
- Educational value as a negative-example archive.
- Explicitly documents that formatting and code cleanliness are not priorities.

**Weaknesses**
- Intentional anti-patterns.
- Inconsistent formatting and style.
- Not suitable as reference-quality implementation code.

**Findings**
- The README says formatting is “not required” and may be better left untouched.
- The project positions itself as “Python bad code examples” / “Python antipatterns”.

**Recommendations**
- Keep raw examples, but pair each with:
  - a cleaned version,
  - a list of violated principles,
  - automated lint findings,
  - a short rationale.
- Add per-example metadata: severity, topic, maintainability impact.

### dependencies_and_packaging — 1/10
**Summary**  
There is no visible packaging story for users or contributors. The repository does not present itself as installable, versioned, or dependency-managed software.

**Strengths**
- Minimal repo footprint.

**Weaknesses**
- No visible `pyproject.toml`, `setup.py`, or dependency manifest at the repository root.
- No release packaging strategy.
- No environment bootstrap instructions beyond the README’s general description.

**Findings**
- The visible root listing shows folders and a README, but no standard Python packaging files.
- The repository has “Packages 0” on GitHub.

**Recommendations**
- If the repo is educational only, add a minimal `pyproject.toml` anyway for tooling.
- Add optional dev dependencies for linting, testing, formatting, and sample validation.
- Define supported Python versions.

### error_handling_and_logging — 1/10
**Summary**  
The repository does not present a disciplined approach to error handling or logging. Given the project’s purpose, this is expected, but it still scores poorly against quality criteria.

**Strengths**
- The theme of the repo can help teach what poor handling looks like.

**Weaknesses**
- No repository-level conventions for exceptions.
- No logging strategy.
- No observable standards for recoverability, diagnosis, or operational safety.

**Findings**
- No visible logging policy, logging config, or guidance documents.
- Samples are presented as disasters rather than resilient implementations.

**Recommendations**
- Add a parallel “fixed” set of examples showing:
  - explicit exception types,
  - bounded try/except blocks,
  - structured logging,
  - contextual error messages,
  - fail-fast vs recoverable strategies.

### performance_and_scalability — 2/10
**Summary**  
Performance does not appear to be a design concern here. Because the repo is example-driven and not a system, this is not fatal to its purpose, but it scores low in a code-quality evaluation.

**Strengths**
- Small sample files are easy to inspect manually.

**Weaknesses**
- No benchmarks.
- No profiling guidance.
- No evidence of algorithmic performance review or scalability considerations.

**Findings**
- The repository’s purpose is showcasing bad code, not efficient implementations.
- No performance/tooling artifacts are visible from the repo structure.

**Recommendations**
- Add tags for examples that demonstrate performance anti-patterns.
- For each such example, include complexity notes and an optimized rewrite.

### security_and_compliance — 2/10
**Summary**  
The repo is careful about privacy of contributed examples, which is the main positive. As a software repository, though, it does not show security practices, policies, or automated checks.

**Strengths**
- README explicitly instructs contributors to remove anything that could violate security requirements, corporate rules, or licenses.
- Encourages de-identification of contributed code.

**Weaknesses**
- No visible security tooling.
- No policy for secrets scanning, dependency auditing, or vulnerability management.
- No compliance posture beyond contribution hygiene.

**Findings**
- The privacy section is concrete and useful.
- No visible security automation or governance files are present.

**Recommendations**
- Add secret scanning / dependency audit tooling.
- Add a SECURITY.md file and contribution rules for sanitizing samples.
- Define allowed / prohibited content for submissions.

### testing_and_reliability — 1/10
**Summary**  
There is no visible testing or reliability strategy. For a real codebase this would be a major deficiency.

**Strengths**
- None visible at repository level.

**Weaknesses**
- No visible `tests/` directory at the root listing.
- No CI evidence in the visible root structure.
- No reliability criteria or regression expectations.

**Findings**
- The repo is not framed as software meant to behave correctly; it is framed as a collection of broken examples.
- No testing conventions are advertised.

**Recommendations**
- Add snapshot or golden-file tests for example classification and metadata.
- Add CI to validate example manifests, markdown consistency, and sanitized content.
- Add “fixed” counterparts with executable tests that prove the correction.

### tooling_and_developer_experience — 3/10
**Summary**  
Developer experience is acceptable only at the level of browsing examples. As a repository to maintain and evolve, it lacks the standard tooling expected for productive collaboration.

**Strengths**
- Simple repo layout.
- Clear README intent.
- Contribution idea is understandable.

**Weaknesses**
- No visible lint/format/test automation in the root structure.
- No contributor workflow or local bootstrap process.
- No issue templates, code ownership, or quality gates visible from the repository surface.

**Findings**
- The README recommends an external linter (`wemake-python-styleguide`), which is a useful directional signal.
- The repository itself does not appear to enforce these tools.

**Recommendations**
- Add pre-commit hooks.
- Add CI checks for formatting, markdown, link validation, and example manifest validity.
- Add a contributor guide with sample submission format.

## Final assessment
As a **teaching repository of anti-patterns**, the project is understandable and even useful. As a **software codebase**, it scores very low almost across the board. The biggest issue is not merely the absence of tests or packaging; it is that the repository’s explicit goal is to preserve poor-quality examples rather than to embody maintainable engineering practice.

That means the fair conclusion is:

- **Useful as a cautionary reference**  
- **Poor as an example of production software quality**

## Suggested next step
The strongest improvement would be to evolve the repo from “bad code gallery” into a **paired-learning repository**:
1. keep the bad example,
2. add the corrected version,
3. explain the violated rule,
4. show linter/test output,
5. document the recommended fix.
