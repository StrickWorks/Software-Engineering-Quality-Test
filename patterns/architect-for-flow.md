# Architect for flow

## Context

* These notes are part of a broader set of [principles](../principles.md)
* This pattern is closely related to the [deliver little and often](little-and-often.md) pattern

## The pattern

Technical design choices should primarily optimise for rapid, reliable delivery and operations.

One key consideration is how best to break the system down into independent services/components.

## Benefits

In high level terms, systems which are designed to maximise rapid, reliable delivery and operations:
* Are cost efficient: teams don't waste their time fighting the tools or working with difficult architectures
* Improve business agility: these systems allow teams to respond more quickly to changes
* Improve reliability: these systems are easier to understand, which leads to fewer failures and shorter recovery times
* Improve team happiness: engineers are happy when the tools they work with let them get on with what they do best

In many cases, building a system as a set of independently running services/components has benefits:
* Multiple components enable parallel development work by multiple teams
* Teams can work at their own cadence
* Changes with each component are easier to reason about and test
* The best tools can be chosen for each job, rather than being hampered by a common set of technologies which need to be adequate for all parts of the system but may only suit some parts of the system well
* Concerns such as scaling, resilience, etc, can be tailored on a per-component basis &mdash; for example avoiding the waste generated by scaling a monolith for the benefit of scaling one small aspect of the system
* It is possible to isolate the impact of catastrophic failure of any individual component
* Self-contained components with clear boundaries of responsibility reduce hand-offs between teams
* Components with clear boundaries of responsibility are more easily replaced
* Components with clear boundaries of responsibility more likely to be reusable &mdash; note: this does not promote building "generic" components &mdash; rather, components that have a clear scope

## Caveats

* This pattern must not compromise quality: automation (including of quality control) is essential for safe implementation of this pattern
* Architectures with multiple moving parts are more complicated. While splitting a system into multiple components is often a good idea, "too many" components can cause more harm than good. There is usually a sweet spot for how many components to break a system down into &mdash; and for small or simple systems a monolith might be better. In distributed systems:
    * There are more failure modes to test, since calls which go over the network can fail in more ways than simple method invocations
    * Versioning becomes a more complicated concern, and additional effort is required to ensure component APIs are compatible as each changes independently
    * Clean domain boundaries are essential for safe implementation of this pattern
    * Comprehensive monitoring and alerting is essential for safe implementation of this pattern
* Components should be built because working in that way gives benefits, not purely because the components might be reused later: if they are later reused, that's even better

## Details

* Split services vertically via [bounded contexts](https://martinfowler.com/bliki/BoundedContext.html) rather than horizontally via technology layers: for example, do not implement dedicated processes to update databases or configuration
* For components to be genuinely independent they need to only interact via their public APIs (e.g. not via a shared database)
* Components should handle the entirety of their bounded context, for example persistence, logic and presentation (though obviously not all components will involve all of these layers)
* This pattern applies to existing services as well as greenfield development projects &mdash; please see Martin Fowler's [StranglerFigApplication blog](https://martinfowler.com/bliki/StranglerFigApplication.html)

TO DO: reference to the NHS UI toolkit (for presentation fragments)

## Examples

TO DO