# How I ship

_Stan Vasilyev. Personal projects; opinions are my own._

These rules come from what went wrong and what worked in my projects. The story
behind them is in [What I learned building with AI coding agents](articles/ai-assisted-engineering-in-practice.md).

- **One owner for each change.** One coordinator carries a change from request to
  production. AI agents help; I stay accountable for what is accepted and released.
- **Small, clear tasks.** An agent gets the outcome and the context it needs, not
  every rule ever written.
- **Specialists for a reason.** Independent review joins when permissions, shared
  data or production change, not for every task.
- **Fast checks tied to the exact commit.** Only checks relevant to the change run.
  No repeated checks unless the code or the environment changes. Passing checks do not prove a
  deploy worked, so releases also need a way to roll back.
- **Watch delivery time.** In BetStan I aim for a build check under 15 minutes and
  a production deploy under 30. When those grow, I simplify the process before
  adding to it.

## Where things run

- **CI:** GitHub-hosted runners by default. When hosted minutes run out, a small
  private platform starts short-lived Azure VMs for jobs, with limits on
  concurrency, lifetime and spend, and a kill switch.
- **Apps:** BetStan runs on Oracle Cloud Infrastructure; the board games run on
  Cloudflare Workers.

[Back to profile](README.md)
