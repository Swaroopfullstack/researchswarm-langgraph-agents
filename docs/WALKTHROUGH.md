# ResearchSwarm walkthrough

## What to demonstrate

A graph composes planner → bounded parallel researchers → deduplicating analyst → writer/critic subgraph → accepted or exhausted outcome.

### Walk through it

1. Upload the NIST PDF through the UI.
2. Ask for a brief about AI risk governance and measurement.
3. Inspect the unique evidence set and final report.
4. Read the graph's Send dispatch: each worker receives its own query, while findings accumulate through an operator.add reducer.
5. Inspect the child graph boundary. It returns only report/score/feedback, not the parent's accumulating findings.
6. Run a fresh user with no indexed evidence. The bounded review loop must end exhausted; it must not claim successful research.

### Read the implementation

[research.py](../src/portfolio/research.py) contains the real fan-out, reducer, compiled child graph and explicit revision bound. The API accepts at most three revision attempts. Source collection is retrieval from owned uploaded material; no web search is claimed. In demo mode the report is explicitly extractive. Live mode uses structured query planning, writing and scoring through the shared model factory.

### Evaluation exercise

Compare this team against a single EvidenceDesk query on the same corpus. Measure relevance, human factuality review, calls/tokens, elapsed time and useful deliverable rate. Multiple agents do not automatically improve a task. A critic score is a rubric signal, not calibrated probability or human approval. Create an expert-labeled set before using scores as a release gate.
