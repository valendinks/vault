---
type: Note
---

# 3D Track Questions

Outstanding leadership questions on the 3D track

I will summarize decisions already taken and product/eng decisions deferred, for us and the team, separately.
Here, these questions 1-3 need answers specifically from MC+AV before we proceed. When can you get back with answers?

Regarding a timeline-to-estimates: 1) kick off both tracks with the whole eng team 2) high level estimates + spike projects to refine understanding 3) more precise estimates after spikes.
I want to kick off with the team on Mon or Tue, with spikes starting right after.

Calendar time - @Henrik Holm and I can discuss and get back to you by EOD, to give a more concrete idea - as in the beginning the two tracks involve the same people.

## 1. Accuracy vs. Smoothness

Meeting summary(AI): Should the external model be *correct* (accurate projections from internal scans) or *pretty* (smooth/clean walls)? The team repeatedly returned to this without resolution. Marina pushed for a concrete answer; the team kept oscillating. This is a strategic call with downstream architectural consequences.

Proposed resolution: In platform architecture, generating derived models gets modularised, so we can iterate on or fork the approaches if needed. We also recognise that for different use-cases different fidelity will be needed eventually - we don't build for that yet, but the platform won't need fundamental rethinking when that comes.
Cost: The eng team will introduce a level of modularisation as part of separating the models into internal/external/envelopes (which is decided, though exact data lineage is tbd), so no extra cost. This will not be a big-bang separate project, but directionally correct work. The exact reconstruction algo will be decided inside the track as a function of short-term product need and eng estimates.

Next step: AV/MC - sounds good? Else write down your stance.

## 2. Edge case investment

Meeting summary(AI): Martin argues optimizing for rare complex buildings isn't worth the cost. Marina and Anders argue the product must support every difficulty level it claims to handle. No resolution.

Next step: AV/MC formulate the stance here in writing to avoid misunderstandings.

## 3. Do we care about internal scan fidelity as "reality capture"?

Meeting summary(AI): Martin raised this as a key alignment question: is the goal to capture the inside of a home as it truly is (since that's the only opportunity to do so), or is "good enough for EPC" sufficient? This has significant implications for engineering investment and what data the company retains long-term.
Also: How does the answer change with/without RGBD capture in addition to parametric?

Next step: AV/MC formulate the stance here in writing to avoid misunderstandings.

## 4. Capturing RGBD/point clouds

This was not discussed in the sessions so far, not directly related to 3D editing of the parametric model. We're not blocked by this, but it keeps coming up.
MC/AV, in your minds, how does this look? Data capture behind-the-scenes? Surfaces in the product to end-users? Incentives to capture? For the next 4-6 months, are you thinking POC for LOI is enough, or becoming a part of the product?
