# Commit message

1. Summary line (<= 50 chars, imperative, no period)
    - Conventional: `type(scope):` short imperative summary
        - Example:
            - `feat(mikrotik): add BGP template for remote1`
            - `fix(inventory): correct wireguard key path`
    - Classic: short imperative summary

2. Blank line

3. Body (wrap ~72 cols)
    - What & why (motivation, intent), not just how
    - Operational / rollout notes (idempotency, backward compat, migration)
    - Risk / rollback strategy
    - Performance / security / compliance considerations if any
    - For config changes: before vs after (concise), affected hosts/groups

4. Optional sections (each separated by blank line)
    - Breaking changes: BREAKING CHANGE: describe impact and required action
    - Related issues / tickets: References: #123 Fixes: #456
    - Co-authors / trailers (Signed-off-by, Reviewed-by, etc.)

5. Footer trailers (standardized, machine-parseable)
    - Signed-off-by: Name <email>
    - Change-Id: (if using Gerrit or similar)
    - Reviewed-by:, Tested-by:, etc. (only if your workflow uses them)


# Content guidelines

- Use imperative mood: "add", "update", "remove", "fix", "refactor"
- One logical change per commit; avoid mixing refactor + feature + formatting
- Explain why a config value changed (business / network rationale)
- Prefer present tense; avoid passive voice
- Avoid noise words: "This commit", "Minor", "Stuff", "Some changes"
- Do not restate diff; add intent missing from code (reason, constraints)
- Reference inventory groups/hosts with consistent naming (e.g. remote1, vpn1)
- For secrets: never include actual key material, just note rotation event


# Types (Contentional Commits baseline)

- _feat_: new functionality
- _fix_: bug/security fix
- _chore_: tooling or maintenance
- _docs_: documentation only
- _refactor_: code/config restructure without behaviour change
- _perf_: performance improvement
- _test_: add or adjust tests
- _ci_: pipeline / automation
- _build_: dependencies, packaging
- _revert_: revert <commit-hash>


# Example (network automation context)

```
feat(mikrotik/ip_firewall_filter): add drop rule for invalid states

Add explicit early drop for connection-state=invalid on core routers
(remote1, remote2, ot-fw) to reduce CPU utilization under scan bursts.
Previously these were traversing later chains increasing latency.

Risk: minimal; rule inserted before existing accept-established.
Rollback: remove rule id comment-tag=auto-invalid-drop.

Before: no dedicated invalid-state drop
After: early drop rule priority 110

Fixes: #42 Signed-off-by: Jane Doe jane@example.org
```


# Minimal template

type(scope): short imperative summary

Motivation / rationale
Key changes (bullet list if several)
Impact / risk / rollback
References / tickets

Fixes: #ISSUE Signed-off-by: Name <email>
