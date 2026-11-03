# Dynamic method assertions (request v2)

Provider: knowlab. Consumer: knowbench. The request discriminator is
`knowbot-test-run/v2`; results, answers and archived case outputs reuse their
unchanged `knowbot-test-run/v1` contracts. v1 remains accepted with its original
final-output-only dynamic assertions.

v2 requires `knowbench/dynamic-methods/v3`. `expectations[].artifact` names either
an explicitly declared final method, or an author method whose expert selection
permits at most one call. It never names an execution instance. An expectation
does not force the planner to select a method.

At evaluation, knowbench resolves the accepted plan and successful Attempt
receipts, validating input lineage and the current completed prefix. Unique
selected intermediate methods may be checked; a missing, unselected, invalidated
or ambiguous output fails its assertion. Final methods use the explicit
`final_outputs` binding. Later unrelated artifacts cannot replace an accepted
result. Intermediate assertions do not promote those methods to final outputs.

No numeric tolerance, row selector, formula, test case or fee behavior changes.
knowlab chooses v2 only for new v3 capability-catalog tests. Already stored v1
requests keep their semantics and digest. Both backends must be deployed before
new v2 requests are enabled; a provider/consumer mismatch is an error, not a
fallback to fewer assertions. Transport timeout, bounded retry and run-id
idempotency use the existing test-run service client and ledger.
