---
title: How to NOT write a GitHub Pull Request Review
---

Part of the job requires reviewing PRs. I always go out of my way to be supportive and helpful when writing these reviews.

But after a recent, particularly challenging review I wondered what it would be like to go the other direction: Condescending, dismissive, arrogant, and so on **(I strongly advise against this!)**.

Who better to write such a review as the condescening, dismissive and arrogant character Q, from Star Trek: The Next Generation?

So here you go:

## Pull Request Review — As Delivered by Q

Well now… what have we here?

A humble utility function, attempting to transcend its station. Admirable. Misguided—but admirable.

@contributor, your enthusiasm has not gone unnoticed. In fact, it has been observed across multiple dimensions. Effort such as yours is… *quaintly human*. And appreciated.

However—let us not confuse *effort* with *precision*.

You see, this function was never destined to be a guardian of security, a sentinel against malware, or a judge of file integrity across the cosmos. No… its purpose is far simpler. Elegantly so.

And yet, in this incarnation, it has… *evolved beyond its design*.

Let us examine the transgressions:

- This function, originally a modest helper, now dares to replicate validation logic already performed elsewhere—specifically within the serializers’ `validate()` methods. Redundancy is not evolution; it is inefficiency.
- It attempts to guard against threats—malware, decompression bombs—concerns now handled by a far more capable entity: the *filescan* service. You are solving problems that have already been… solved.
- It introduces checks—file size, format allowlists—that belong higher in the application’s hierarchy. Not here. Not in this function.

In short: this function has lost its way.

As for the failed pipeline checks… how unfortunate. The logs have vanished into the void. Even I cannot retrieve what no longer exists. You will simply have to try again.

---

### Conclusion

Strip away the excess. Refactor with purpose. Embrace simplicity.

Make this function what it was always meant to be:

*A precise, focused instrument—nothing more, nothing less.*

---

Now then… shall we see if humanity can manage *that*?
