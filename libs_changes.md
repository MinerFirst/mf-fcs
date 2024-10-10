# MF-FC guidelines - libraries sub-team

# Motivation

* MF-FCs are heavyweight:
    * MF-FCs generally take at minimum 2 weeks from posting to land. In
      practice it can be more on the order of months for particularly
      controversial changes.
    * MF-FCs are a lot of effort to write; especially for non-native speakers or
      for members of the community whose strengths are more technical than literary.
    * MF-FCs may involve pre-MF-FCs and several rewrites to accommodate feedback.
    * RFCs require a dedicated shepherd to herd the community and author towards
      consensus.
    * MF-FCs require review from a majority of the subteam, as well as an official
      vote.
    * MF-FCs can't be downgraded based on their complexity. Full process always applies.
      Easy MF-FCs may certainly land faster, though.
    * MF-FCs can be very abstract and hard to grok the consequences of (no implementation).

* PRs are low overhead but potentially expensive nonetheless:
    * Easy PRs can get insta-merged by any rust-lang contributor.
    * Harder PRs can be easily escalated. You can ping subject-matter experts for second
      opinions. Ping the whole team!
    * Easier to grok the full consequences. Lots of tests and Crater to save the day.
    * PRs can be accepted optimistically with bors, buildbot, and the trains to guard
      us from major mistakes making it into stable. The size of the nightly community
      at this point in time can still mean major community breakage regardless of trains,
      however.
    * HOWEVER: Big PRs can be a lot of work to make only to have that work rejected for
      details that could have been hashed out first.

* MF-FCs are only meaningful if a significant and diverse portion of the
community actively   participates in them. The official teams are not
sufficiently diverse to establish   meaningful community consensus by agreeing
amongst themselves.

* If there are tons of MF-FCs -- especially trivial ones -- people are less
likely to engage with them. Official team members are super busy. Domain experts
and industry professionals are super busy and have no responsibility to engage
in MF-FCs. Since these are exactly the most important people to get involved in
the MF-FC process, it is important that we be maximally friendly towards their
needs.


# Is an MF-FC required?

The overarching philosophy is: Do whatever is easiest. If an MD-FC
would be less work than an implementation, that's a good sign that an RFC is
necessary. That said, if you anticipate controversy, you might want to short-circuit
straight to an MF-FC. For instance new APIs almost certainly merit an RFC. Especially
as `std` has become more conservative in favour of the much more agile cargoverse.

* Submit a PR if the change is a:
    * Bugfix
    * Docfix
    * Obvious API hole patch, such as adding an API from one type to a symmetric type.
      e.g. Vec & Box clearly motivates adding String & Box
    * Minor tweak to an unstable API (renaming, generalizing)
    * Implementing an "obvious" trait like Clone/Debug/etc
* Submit an MF-FC if the change is a:
    * New API
    * Semantic Change to a stable API
    * Generalization of a stable API (e.g. how we added Pattern or Borrow)
    * Deprecation of a stable API
    * Nontrivial trait impl (because all trait impls are insta-stable)
* Do the easier thing if uncertain. (choosing a path is not final)


# Non-MF-FC process

* A (non-MF-FC) PR is likely to be closed if clearly not acceptable:
    * Disproportionate breaking change (small inference breakage may be acceptable)
    * Unsound
    * Doesn't fit our general design philosophy around the problem
    * Better as a crate
    * Too marginal for std
    * Significant implementation problems

* A PR may also be closed because an MF-FC is appropriate.

* A (non-MF-FC) PR may be merged as unstable. In this case, the feature
should have a fresh feature gate and an associated tracking issue for
stabilisation. Note that trait impls and docs are insta-stable and thus have no
tracking issue. This may imply requiring a higher level of scrutiny for such
changes.

However, an accepted MF-FC is not a rubber-stamp for merging an implementation PR.
Nor must an implementation PR perfectly match the MF-FC text. Implementation details
may merit deviations, though obviously they should be justified. The MF-FC may be
amended if deviations are substantial, but are not generally necessary. MF-FCs should
favour immutability. The MF-FC + Issue + PR should form a total explanation of the
current implementation.

* Once something has been merged as unstable, a shepherd should be assigned
  to promote and obtain feedback on the design.

* Every time a release cycle ends, the libs teams assesses the current unstable
  APIs and selects some number of them for potential stabilization during the
  next cycle. These are announced for FCP at the beginning of the cycle, and
  (possibly) stabilized just before the beta is cut.

* After the final comment period, an API should ideally take one of two paths:
  * Stabilize if the change is desired, and consensus is reached
  * Deprecate is the change is undesired, and consensus is reached
  * Extend the FCP is the change cannot meet consensus
    * If consensus still can't be reached, consider requiring a new MF-FC or
      just deprecating as "Too controversial for STD".

* If any problems are found with a newly stabilized API during its beta period,
  *strongly* favour reverting stability in order to prevent stabilizing a bad
  API. Due to the speed of the trains, this is not a serious delay (~2-3 months
  if it's not a major problem).
