# What I learned building with AI coding agents

_Stan Vasilyev. Personal projects; opinions are my own._

AI coding agents have made me much faster. An idea for a board game became a
playable multiplayer build in under four hours. Shortly before that, the same
tools left a simple fix unreleased for three weeks.

Both happened in my own projects, with the same tools. This article is about
what made the difference. These are case studies from personal projects, not a
controlled productivity study.

## BetStan: a UI fix that could not ship

I started [BetStan](https://github.com/vasilyevstan/betstan) in 2023 to learn
TypeScript and microservices. It is a sports-betting **simulation**, with no real
money or real match feeds. In 2026 I brought it back as a place to practise
AI-assisted and multi-agent engineering.

I asked for something simple: bring the services back up and apply a few UI
fixes, such as the page header. Production was already down; the last successful
deploy had been five days earlier.

The next successful production deploy came **three weeks** after my request:
26 days between successful deploys, with eight failed attempts in that interval.
The UI fixes waited behind the recovery. Two and a half weeks in, I was
documenting how long recovery was taking and how long the services had been
unavailable.

### Verifying the verification

The agents were busy the entire time. They reported blockers like these:

- A service took 321 seconds to start, but its deploy stage allowed 240.
- Recovery first had to prove it could hand over a database lock before the lock
  expired.
- A reviewer agent found gaps in the recovery checks. Fixing them exposed a bug
  where the checker's own temporary files made its clean-workspace check fail.

Some of these were real defects. Many were checks guarding other checks. Every
fix added more steps, and each new step could fail on its own. In the two weeks
before the clean-up, the pull-request build check took a median of 51 minutes,
and a few earlier runs took around seven hours. At one point the delivery flow
had reached step 57.

### Losing control, and my part in it

I repeatedly asked for fewer review gates and steps, pointing out that 57 steps
for a small UI fix made no sense and that a UI fix should not take a day to
deploy.

The agents kept going back to their recovery path and their proofs. Two weeks in,
I even asked for a simplifier agent with authority to stop work. It became one
more role, and I later folded it into the coordinator. Almost three weeks in, an
agent's working note still prioritized restoration over the UI work. That was the
opposite of what I had asked for.

By then I had lost control of my own project and was desperate to get it back.
I kept giving instructions and hoping that the next one would make a difference.
It did not, and it felt hopeless.

The honest part is that I had built this. Every rule, review and specialist had
been added for a reason. Together they outranked my latest instruction. The
agents were following the process faithfully. The process was the problem.

### Cutting it back

Hoping for the next attempt to work was not enough. I set two targets: a build
check under 15 minutes and a production deploy under 30 minutes. Then we changed
the process:

- [Fixed](https://github.com/vasilyevstan/betstan/pull/973) the deploy health check
  that restarts from an earlier reboot had been blocking.
- [Removed about 2,600 lines](https://github.com/vasilyevstan/betstan/pull/988) of
  agent instructions, including mandatory agreement between models and reviews of
  reviews.
- [Merged 13 agent roles into 9](https://github.com/vasilyevstan/betstan/pull/1007)
  with one delivery coordinator, and replaced eight duplicate test workflows with
  one.

Simplifying did not mean removing safety. Tests tied to the released commit,
independent review of sensitive changes and a way to recover all stayed.

### The structure that works now

The simpler setup has one delivery coordinator, with development, testing,
design and operational specialists available when needed. Not every task needs
all of them.

- **One owner.** The coordinator carries a change from request to production.
  I remain accountable for what is accepted and released.
- **Specialists only for a gap.** Security review joins when permissions or shared
  data change, not for every task.
- **Fast checks tied to the commit.** Only checks relevant to the change run;
  no repeated checks unless the code or the environment changes.
- **Stop and simplify** when keeping the process going becomes the work.

```mermaid
flowchart LR
    R["Request"] --> C["Coordinator: one owner"]
    C --> W["Bounded work, with specialists if needed"]
    W --> K["Fast checks on the exact commit"]
    K --> P["Production"]
```

### What changed

| Measure | Before (window measured) | After (following full week) |
| --- | --- | --- |
| Production deploys | 26 days between successes; 8 failed attempts | 11 of 12, median 12 min |
| Production data rollouts | 17 of 42 succeeded (4 weeks) | 33 of 36, median 13 min |
| Pull-request build check | median 51 min (2 weeks) | median 3.6 min |
| Pull request, opened to merged | median 1.1 h, p90 6.8 h (1 week) | median 0.1 h, p90 0.7 h |

Two days after the recovery, I asked for a player-profile page one evening. It
was in production about 7.5 hours later. A wrong bet count I reported the next
morning was fixed and released the same day.

These figures come from GitHub Actions and pull-request history in one person's
project. The baseline windows differ in length, as shown; the comparison uses
the following full week after recovery. The clean-up also included restoring
production and fixing other delivery problems, so these are before-and-after
observations, not proof that one change caused the improvement. Here, p90 means
90% of pull requests took no longer than the stated time to merge.

**Lesson:** telling the agents to simplify did not change the process they were
running. I had to change the process itself, and adding another agent to manage
it was not the answer.

## The same tools, different kinds of judgment

### Moving BetStan from Azure to OCI

To cut costs I moved BetStan from Azure to Oracle Cloud Infrastructure. Scheduled
jobs and queued messages could still change data during the move. One attempt to
stop writes revoked messaging permissions and broke the services instead. The
[revised procedure](https://github.com/vasilyevstan/betstan/pull/205) paused
message routing and set the restart order.

**Lesson:** a plan that reads well is not evidence. Decide in advance what proof
lets you allow writes again.

### StanStock: old forecasts are history, not advice

[StanStock](https://github.com/vasilyevstan/stanstock) is my local stock-research
app. It forecasts prices for a target date. It is not a broker.

Once a forecast's target date passed, a safety rule correctly stopped treating it
as current. But the page then showed only an error, and the past forecast
disappeared. I could no longer look back at what the model had predicted.

The [fix](https://github.com/vasilyevstan/stanstock/pull/88) shows old forecasts
clearly labelled as dated history. They never appear in today's buy shortlist, and
future-dated or corrupt output is still hidden.

**Lesson:** a safety rule should block the risky use (acting on old advice), not
the useful one (learning from past predictions).

### TrackStan: a map can look right and still be wrong

[![TrackStan public traffic map](../assets/trackstan.png)](https://trackstan.xyz)

[TrackStan](https://github.com/vasilyevstan/LiveTrafficStan) shows live air and
marine traffic on a map. One data provider requires a wait before you ask again.
That wait had to survive the user moving the map or pausing updates; asking too
soon breaks the provider's rules. Some ship positions could also carry future
timestamps and make old data look fresh.

The [fix](https://github.com/vasilyevstan/LiveTrafficStan/pull/603) keeps the wait
across map moves and pauses, and rejects future-dated positions. Tests with a
controlled clock make both cases repeatable.

**Lesson:** markers on the screen do not prove the data is fresh or that you
respected the provider's rules. Test the rules directly.

### Home Automation: a safety guard that was too strict

In my private home-energy project, an AI assistant can propose a limit on solar
output. A separate controller with fixed rules decides whether to send it, and
reads the device back afterwards.

The first supervised attempt stopped before sending anything. The guard mistook
normal changes in live power for a change in the device's settings. The fix
defined it precisely: settings and limits are guarded; live power is just an
observation. A later supervised trial capped and restored output correctly. That
is one trial, not proof of long-term reliability.

**Lesson:** when a guard is too strict, define it more precisely. Do not give the
AI more authority.

### Catanastan: polish is not correctness

[Catanastan](../projects/catanastan-edge-multiplayer.md) is my multiplayer board
game. Players reconnect and browsers retry, but a retried action must never count
as a second move. The server checks every action and sends each player only what
they are allowed to see. A polished 3D board does not solve any of that.

**Lesson:** in multiplayer games, the hard work is invisible. Decide early who
owns the game state.

## Zombies in da House: fast through iteration

One evening I started exploring whether we could make our own version of a board
game I liked. Three hours and 44 minutes later, the first playable
multiplayer build was committed. The first cloud release followed the next
morning. The three boards below show early milestones; the
[illustrated game history](../projects/zombies-in-da-house-edge-multiplayer.md)
follows sixteen snapshots through v28.

| First playable build | The house moves into the woods | v19: log cabin and van bay |
| --- | --- | --- |
| ![First playable build: a simple tiled house on a flat green board](../assets/zombies-history/01-lastlight-board.jpg) | ![Forest floor, furnished house and objectives sidebar](../assets/zombies-history/07-living-woods-board.jpg) | ![Log-cabin walls and a marked van bay](../assets/zombies-history/11-v19-board.jpg) |

_Screenshots were rebuilt later from the original release commits, not captured at
the time. [Read the full illustrated history](../projects/zombies-in-da-house-edge-multiplayer.md#first-playable-build-lastlight-house)._

The first playable version was only a beginning. Most changes came from playing
it: solo players started in the middle and could not move on the first turn;
hidden items near the fireplace were invisible; carried items looked unnatural.
Each round of play produced a new list of fixes.

**Lesson:** the speed comes from short loops with a person playing the game, not
from trying to specify the whole game up front.

## What I keep doing

The working rules I took from all this are in [How I ship](../how-i-ship.md).
