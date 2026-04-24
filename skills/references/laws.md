# Laws of UX — Full Reference

Source: https://lawsofux.com/

---

## 1. Aesthetic-Usability Effect

**Core:** Users often perceive aesthetically pleasing design as more usable.
**Detail:** Beautiful interfaces create a positive emotional response that makes users more tolerant of
minor usability issues. This can mask real problems during testing — users rate polished designs higher
even when usability is equivalent to plainer alternatives.
**Watch for:** Over-reliance on aesthetics to paper over poor information architecture.
**QA note:** Don't let visual polish inflate usability scores in testing.

---

## 2. Choice Overload (Paradox of Choice)

**Core:** Too many options overwhelm users and reduce decision quality and satisfaction.
**Detail:** Sheena Iyengar's jam study showed fewer options led to more purchases. In UI, this means
menus, filter panels, and settings pages with too many options cause abandonment.
**Watch for:** Filter panels with 20+ options, navigation with too many top-level items, settings sprawl.
**Conflicts with:** Completeness — sometimes users need many options. Solution: progressive disclosure.

---

## 3. Chunking

**Core:** Breaking information into meaningful groups improves comprehension and memory.
**Detail:** Users process and remember grouped information better than continuous streams. Applies to
forms (group related fields), content (section headers), and navigation (grouped links).
**Watch for:** Long ungrouped forms, walls of text, flat navigation lists.

---

## 4. Cognitive Bias

**Core:** Systematic errors in thinking affect how users perceive and judge interfaces.
**Detail:** Includes anchoring (first number seen shapes judgment), confirmation bias (users see what
they expect), and framing effects (how options are presented affects choice).
**Watch for:** Misleading defaults, dark patterns that exploit anchoring, deceptive pricing layouts.

---

## 5. Cognitive Load

**Core:** Minimize the mental effort required to use the interface.
**Detail:** Three types: intrinsic (complexity of the task itself), extraneous (complexity added by
poor design), germane (effort that builds useful mental models). Only germane load is acceptable.
**Watch for:** Unexplained jargon, inconsistent interactions, hidden affordances, dense layouts.
**This is the master law** — if cognitive load is high, most other laws will also be violated.

---

## 6. Doherty Threshold

**Core:** Productivity peaks when a computer and its users interact at ≤400ms response time.
**Detail:** Delays over 400ms break flow and require users to re-engage attention. Over 1 second,
users expect a loading indicator. Over 10 seconds, users will leave.
**Watch for:** Missing skeleton screens, no progress indicators, slow form submissions without feedback.

---

## 7. Fitts's Law

**Core:** The time to acquire a target is a function of its size and distance from the starting point.
**Detail:** Small or far-away targets take longer to click/tap. Mobile CTAs must be finger-sized (min
44×44px). Primary actions should be close to where the user's attention already is.
**Watch for:** Tiny close buttons, mobile CTAs smaller than 44px, destructive actions placed next to
primary actions, targets in screen corners on mobile.

---

## 8. Flow

**Core:** Users enter a state of deep focus when challenge and skill are balanced.
**Detail:** Csikszentmihalyi's concept applied to UX: interfaces that are too easy bore users; too
hard, they abandon. Flow is broken by interruptions, forced context switches, and unexpected errors.
**Watch for:** Unnecessary modals, forced registration walls, unexpected state changes.

---

## 9. Goal-Gradient Effect

**Core:** People accelerate effort as they get closer to completing a goal.
**Detail:** Showing progress (even artificially boosted — "starter points") motivates completion.
Progress bars, step indicators, and completion percentages all leverage this.
**Watch for:** Multi-step flows with no progress indicator, profile completion without a progress bar.

---

## 10. Hick's Law

**Core:** Decision time increases logarithmically with the number and complexity of choices.
**Detail:** T = b × log₂(n+1). Doubling the options doesn't double decision time — but it does
meaningfully slow users. Simplify choices, use progressive disclosure, and provide smart defaults.
**Watch for:** Navigation bars with 10+ items, forms with many required fields shown at once.
**Conflicts with:** Choice Overload (#2) — both say similar things, but Hick's Law is more
mathematical. Use together to justify navigation simplification.

---

## 11. Jakob's Law

**Core:** Users spend most of their time on other sites, so they expect yours to work the same way.
**Detail:** Convention beats creativity for core interactions. Hamburger menus, shopping carts top-right,
search at the top — these are conventions users rely on. Deviation requires justification.
**Watch for:** Novel navigation paradigms without clear benefit, non-standard form patterns,
unusual icon choices for common actions.

---

## 12. Law of Common Region

**Core:** Elements within a defined boundary are perceived as grouped.
**Detail:** Cards, boxes, borders, and background color changes create perceived groups. Even a subtle
background change is enough to create grouping.
**Watch for:** Related elements not enclosed in a shared region, unrelated elements sharing a card.

---

## 13. Law of Proximity

**Core:** Objects near each other are perceived as related.
**Detail:** The most powerful gestalt grouping principle. Labels should be closer to their inputs than
to adjacent labels. Related actions should be adjacent. Spacing is meaning.
**Watch for:** Labels equidistant from two inputs, action buttons separated from the content they act on.

---

## 14. Law of Prägnanz (Law of Simplicity)

**Core:** People perceive complex or ambiguous forms in the simplest possible way.
**Detail:** Users will simplify what they see to match the simplest interpretation. Overly complex
layouts get "simplified" in the user's mind — which may mean important elements get ignored.
**Watch for:** Layouts with competing visual hierarchies, icon sets that look like decoration.

---

## 15. Law of Similarity

**Core:** Similar-looking elements are perceived as having the same function.
**Detail:** Color, shape, size, and texture similarity create implicit groupings. If two things look
alike, users expect them to behave alike. Breaks in similarity signal distinction.
**Watch for:** Destructive and constructive actions with similar styling, different interactive
states (disabled/enabled) that look too similar.

---

## 16. Law of Uniform Connectedness

**Core:** Elements connected by lines or other visual links are perceived as related.
**Detail:** Connecting lines, arrows, and shared visual properties (like color) imply relationship.
Stronger than proximity or similarity.
**Watch for:** Connecting lines between unrelated elements, flow diagrams that imply wrong relationships.

---

## 17. Mental Model

**Core:** Users have an internal model of how a system works, built from prior experience.
**Detail:** When your UI violates a user's mental model, errors and confusion follow. The best
interfaces align with or gently expand the user's existing model.
**Watch for:** New metaphors without explanation, non-standard terminology for common concepts,
workflows that contradict how users do things in the real world.

---

## 18. Miller's Law

**Core:** The average person can hold only about 7 (±2) items in working memory at a time.
**Detail:** Often misapplied as "max 7 items per list" — the real point is that chunking and grouping
allow users to process more by treating groups as single items.
**Watch for:** Navigation with 9+ flat items, forms with many ungrouped fields, dashboards with many
equal-weight widgets.

---

## 19. Occam's Razor

**Core:** Among competing explanations or designs, prefer the simplest.
**Detail:** Every element in a UI should earn its place. If removing an element doesn't change the
function, remove it. Applied to design, it favors minimalism over decoration.
**Watch for:** Decorative elements that add visual noise, redundant labels, unnecessary steps in flows.

---

## 20. Paradox of the Active User

**Core:** Users jump in and start using systems without reading documentation or instructions.
**Detail:** Users are goal-oriented and impatient. They won't read onboarding guides, tooltips, or
help text unless they're stuck. Design must be self-evident.
**Watch for:** Designs that rely on users reading instructions, complex features with no inline guidance,
empty states with no action prompts.

---

## 21. Pareto Principle (80/20 Rule)

**Core:** 80% of effects come from 20% of causes.
**Detail:** Applied to UX: 80% of users use 20% of features. Prioritize and surface the most-used
features. Also useful for audit prioritization: fix the 20% of issues causing 80% of friction.
**Watch for:** Feature-heavy navigation that buries the primary action, equal visual weight for all features.

---

## 22. Parkinson's Law

**Core:** Work expands to fill the time available for its completion.
**Detail:** Applied to forms and tasks: if you give users unlimited space (e.g., a large text area),
they'll fill it. Set appropriate constraints and time limits to guide behavior.
**Watch for:** Open-ended forms where brevity is desired, task flows with no time guidance.

---

## 23. Peak-End Rule

**Core:** People judge an experience primarily by how they felt at its most intense point and at its end.
**Detail:** A frustrating checkout flow with a delightful confirmation screen is remembered more
positively than a smooth flow with an anticlimactic ending. Design your peaks and endings deliberately.
**Watch for:** Weak success states, error states with no recovery path, long flows with no positive
reinforcement at the end.

---

## 24. Postel's Law (Robustness Principle)

**Core:** Be liberal in what you accept from users; be conservative in what you send to them.
**Detail:** Accept phone numbers with or without dashes, spaces, parentheses — normalize on the
backend. Don't make users conform to your system's format requirements.
**Watch for:** Rigid form validation ("must be in format XXX-XXX-XXXX"), case-sensitive search,
no tolerance for typos in identifiers.

---

## 25. Selective Attention

**Core:** Users focus only on elements relevant to their current goal; everything else is tuned out.
**Detail:** Banner blindness is a classic example. Users in task mode ignore navigation, promotions,
and anything off the critical path. Don't bury important information in banners.
**Watch for:** Critical error messages styled like marketing banners, important info in sidebars
when users are focused on a central task.

---

## 26. Serial Position Effect

**Core:** Items at the beginning and end of a list are remembered better than those in the middle.
**Detail:** Primacy effect (first items) and recency effect (last items). The middle of a list is
the dead zone for attention and recall.
**Watch for:** Primary CTAs buried in the middle of navigation, important features in the middle of
feature lists, key information sandwiched between other content.

---

## 27. Tesler's Law (Law of Conservation of Complexity)

**Core:** Every application has an inherent amount of complexity that cannot be removed — only shifted.
**Detail:** You can move complexity from the user to the system (smart defaults, automation) or from
the system to the user (manual configuration). The goal is to put complexity where it can best be handled.
**Watch for:** Overly simplified UIs that push complexity onto power users, expert tools with no
advanced configuration.

---

## 28. Von Restorff Effect (Isolation Effect)

**Core:** When multiple similar objects are present, the one that differs from the rest is most likely
to be remembered.
**Detail:** Use contrast deliberately to draw attention to primary CTAs, important warnings, or key
information. Overuse of contrast destroys the effect.
**Watch for:** Primary CTA that doesn't visually stand out, three equally prominent buttons, warning
states that aren't visually distinct from normal states.

---

## 29. Working Memory

**Core:** The brain's temporary storage is small and fragile — it degrades quickly and under load.
**Detail:** Multi-step flows that require users to remember information from earlier steps violate
working memory. Show a summary, use progressive disclosure, and don't make users hold context.
**Watch for:** Forms that show a value on one screen and ask about it on the next, wizards without
a summary panel, no persistent display of cart/order state.

---

## 30. Zeigarnik Effect

**Core:** People remember uncompleted tasks better than completed ones.
**Detail:** This is why progress bars and "your cart has items" notifications work. Incomplete states
create psychological tension that motivates return and completion.
**Watch for:** No draft-saving, no "resume where you left off", no abandoned cart reminders.

---

## Known Law Conflicts

| Tension                                        | Resolution                                                    |
| ---------------------------------------------- | ------------------------------------------------------------- |
| Choice Overload (#2) vs. completeness          | Progressive disclosure: show defaults, allow expansion        |
| Occam's Razor (#19) vs. Tesler's Law (#27)     | Ask: who bears the complexity — user or system? Prefer system |
| Jakob's Law (#11) vs. innovation               | Innovate in content, not core navigation patterns             |
| Aesthetic-Usability (#1) vs. honest UX testing | Strip visual polish for usability tests to get cleaner signal |
| Serial Position (#26) vs. logical ordering     | Reorder for psychology, not just logic — test both            |
