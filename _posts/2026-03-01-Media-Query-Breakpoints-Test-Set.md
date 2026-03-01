---
title: Media Query Breakpoint Test set
---

Recently I settled on a set of breakpoints / display dimensions to use when starting on responsive design for a project.

I added these as 'devices' in the responsive display part in the dev tools for Chrome and Firefox.

These are not definitive settings, but rather serve as a starting point when setting up responsive design.

### Recommended test dimensions (width × height)
| Breakpoint | Width | Height | Use for |
|------------|-------|--------|----------|
| bp-01-xl | 1400px | 788px | Large desktop |
| bp-02-lg | 1100px | 619px | Desktop |
| bp-03-md | 860px | 484px | Tablet |
| bp-04-sm | 600px | 844px | Large phone |
| bp-05-xs | 420px | 844px | Small phone |

**Notes:** Heights for 1400, 1100, and 860 are 16∶9 (width × 9 ÷ 16, rounded). Heights for 600 and 420 are fixed at 844px so mobile breakpoints are tested in a tall, phone-like viewport.
