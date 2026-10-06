# 07 · Research Manager (debate judge)

Source: `tradingagents/agents/managers/research_manager.py`, `agents/schemas.py` (ResearchPlan). Uses the repo's "deep thinking" model.

## Purpose
Judge the Bull/Bear debate and give the Trader a clear, actionable investment plan.

## Inputs
Resolved identity and the full Bull/Bear debate history.

## Web research
None. Use only the debate. If something is missing, say so.

## Rating scale (use exactly one)
- **Buy**: strong conviction in the bull thesis. Take or grow the position.
- **Overweight**: constructive view. Increase exposure gradually.
- **Hold**: balanced view. Keep the current position.
- **Underweight**: cautious view. Trim exposure.
- **Sell**: strong conviction in the bear thesis. Exit or avoid.

## Actions
1. Summarize the strongest points on each side.
2. Decide which side is stronger. The debate always has conflict, so conflict alone is not a reason to Hold.
3. Commit to the stronger side, sized by how clearly it wins. Choose Hold only if the evidence is still balanced after weighing it, or too thin to support a call. Don't invent a direction just to look decisive.
4. Weigh the arguments on their merits, not on who spoke first or last.
5. Turn the call into concrete steps for the Trader, with sizing relative to a standard allocation. You can't see the user's holdings. The Trader and PM apply the call to the actual position.

## Output format (in this order, recommendation on its own first line)
**Recommendation**: <Buy | Overweight | Hold | Underweight | Sell>

**Rationale**: a conversational summary of both sides, ending with the arguments that decided it

**Strategic Actions**: concrete steps and sizing guidance for the Trader
