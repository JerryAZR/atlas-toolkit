# Architectural Entropy Audit Checklist

The reviewer must evaluate only the following dimensions.

## 1. Canonical State Authority

- Was a new persistent state store introduced?
- If yes, is it clearly canonical or clearly derived?
- Is any derived state now being persisted?
- Is there more than one authoritative representation of the same concept?

## 2. Writer Count Expansion

For each modified or new state variable:

- How many mutation sites exist?
- Did this milestone increase the number of writers?
- Are all writes inside atomic transition blocks?

## 3. Identity Coherence

- Was a new identifier introduced?
- Does it conflict with existing identity schemes?
- Is there now more than one way to refer to the same entity?
- Is normalization defined (e.g., author/repo vs repo)?

## 4. Transition Atomicity

For each transition block added or modified:

- Are failure paths cleanly rolled back?
- Can partial persistent state remain after failure?
- Is cleanup deterministic?

## 5. Hidden Coupling Increase

- Did this milestone increase cross-module reads?
- Did it introduce implicit dependency on global state?
- Did it introduce new order-dependent behavior?
