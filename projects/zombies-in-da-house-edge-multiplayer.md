# How Zombies in da House evolved

**[Play the live demo](https://zombie-in-da-house.syntal.workers.dev)** · private source repository

It started with a question: _do you know the board game «Зомби в доме», and can we make our own version of it?_ The answer became an original browser tabletop in the same spirit — find the vehicle key and the fuel, get back to the van, and if you fall, play for the house — with its own art, rules text and code. The first playable multiplayer build was committed that evening. Less than three weeks later the game had reached v28, after 163 commits on `main` and 66 merged pull requests.

Every picture on this page comes from the real code of its day. Each snapshot was checked out, built from scratch and played again with the same seed; nothing is a mock-up. See [How these pictures were made](#how-these-pictures-were-made).

![Sixteen landing pages in a four-by-four grid, one per snapshot, from the green Lastlight House to the winged Warden of v28](../assets/zombies-history/00-contact-sheet.jpg)

## At a glance

| Snapshot | Changes | Highlights |
| ---------------------------- | ---------------------------------------- | ------------------------------------------------------------------------------- |
| Lastlight House, [`f6e57cf`] | first commit | A playable multiplayer table: shared device or online room, spin to move. |
| Blue miniature, [`14b2572`] | [#1], [#3] | The name, a moonlit furnished cutaway, invitations and the first cloud release. |
| Solo nights, [`bb9056f`] | [#7] | Play alone with up to four bot companions; your rooms on the homepage. |
| Timber house, [`48b47c9`] | [#9] | A decaying wooden house, bots in every mode, the combat dial. |
| Blender cast, [`7439b82`] | [#11], [#13] | Detailed survivors and monsters modeled in Blender. |
| Haunted grounds, [`75db377`] | [#14] | Compact hero selection and explicit turn controls. |
| Living woods, [`2b5cbea`] | [#16], [#19], [#21] | Baked woods and trees, a burning hearth, house totals, a compact dock. |
| Getaway bus, [`096a1f2`] | [#23] | Compact setup, one Menu button, monsters rising from the floor, the hippie bus. |
| v16, [`ff88425`] | [#27] | The original's weighted spinner, Autopilot, haunted gates. |
| v17–v18, [`ff6dda9`] | [#29], [#31] | Arrival and departure cutscenes, the Hall of Fame, rescues, the Bazooka. |
| v19, [`06749f5`] | [#33] | The van bay, a log cabin, a sunken road, four cutscenes. |
| v20–v23, [`1389669`] | [#38], [#40], [#42], [#44] | Roster hearts, a last stand, mood-matched music, discs on every hidden cell. |
| v24–v25, [`0d1ae73`] | [#48], [#50] | The rulebook map, a Blender gravel road, clean music handoffs. |
| v26, [`84bd0de`] | [#52], [#56], [#60], [#64], [#66], [#68] | Smarter bots, a smooth entrance, a scene for every fall, an aerodynamic van. |
| v27, [`06dba2a`] | [#70] | IDDQD and IDKFA, a winged Warden, checkmarks for survivors who reach the van. |
| v28, [`0504769`] | [#72] | No cheated games in the Hall of Fame, readable supplies, a Warden that walks. |

## First playable build — Lastlight House

Snapshot [`f6e57cf`], the first commit.

The very first build was called **Lastlight House**: a pale green, low-poly table with pastel rooms, face-down tiles and a few white trees. You set the table for a shared device or an online room, named two survivors and pressed **Spin movement**. The van is broken; find the key and the fuel. It looked like a prototype, but the engine underneath was already serious: authoritative multiplayer, inventory and rules on the server.

<p>
  <img src="../assets/zombies-history/01-lastlight-landing.jpg" width="49%" alt="Lastlight House landing page: a Set the table form beside a pale green low-poly board">
  <img src="../assets/zombies-history/01-lastlight-board.jpg" width="49%" alt="Lastlight House board with pastel rooms, beige face-down tiles, white low-poly trees and a Spin movement button">
</p>

## The blue miniature

Snapshot [`14b2572`] · [#1] and [#3], promoted in [#2].

Overnight the game got its name, **Zombie in da House**, and its look: a blue, moonlit miniature of the house in cutaway. [#1] added invitations — **Play with friends**, **Join with an invitation** or pass a **Shared device** — for two to five players in private rooms, with no accounts. [#3] furnished it: a study, kitchen, bedroom and living room with searchable furniture, and windows you can climb through, all from an original 61-prefab model kit. This snapshot was the first cloud release.

<p>
  <img src="../assets/zombies-history/02-blue-miniature-landing.jpg" width="49%" alt="Zombie in da House landing page: a small blue moonlit house miniature with Play with friends, Join with an invitation and Shared device">
  <img src="../assets/zombies-history/02-blue-miniature-board.jpg" width="49%" alt="The blue furnished cutaway board with blue trees, drifting clouds and a Begin move button">
</p>

## Solo nights

Snapshot [`bb9056f`] · [#7], promoted in [#8].

The same afternoon brought a real one-survivor night: **Play alone** with zero to four bot companions, five survivors at most. Bots make one paced decision at a time through the same strict server path as people. The homepage gained **Your rooms** to resume or delete the rooms you created, and the landing page finally showed a roofed haunted house. The setup still used the first, blocky figures.

<p>
  <img src="../assets/zombies-history/03-solo-nights-landing.jpg" width="49%" alt="Landing page with a roofed blue haunted house, a van outside and the Your rooms panel">
  <img src="../assets/zombies-history/03-solo-nights-setup.jpg" width="49%" alt="Your own night setup with a blocky hero preview and a bot companions choice">
</p>

## The timber house

Snapshot [`48b47c9`] · [#9], promoted in [#10].

The brief: a frightening, decaying wooden house with natural green grass, a scary title and Mortal Kombat-style ability showcases on the selection screen. The house became weathered timber in the grass. Bots could now join every kind of table, fights got the original game's combat arrow as a dial, and Cloudflare Web Analytics began counting page views. The wordmark in this snapshot reads “ZONBIES in da HOUSE”; [#13] changed it to “ZOMBIES” the next day. Two independent reviews accepted the house but sent the cast back for a rebuild — which is what happened next.

![The decaying timber house on a round patch of grass, with the van parked beside the porch](../assets/zombies-history/04-timber-house-landing.jpg)

## The Blender cast

Snapshot [`7439b82`] · [#11] and [#13], promoted in [#12].

Every figure was rebuilt in Blender: five adult survivors with 12,600–13,600-vertex heads, baked face textures, styled hair and role props (a blade, a carbine, a first-aid case, a riot shield), and four new monsters — a hound, a crawler, the Warden and a Shambler. The exported kit weighed 27.4 MB, over Cloudflare's 25 MiB per-file limit, so it was mesh-quantized to 18.9 MB. On the landing page the survivors now wait on the porch.

![The timber house at night with the Blender survivors sitting on the porch and the corrected ZOMBIES in da HOUSE wordmark](../assets/zombies-history/05-blender-cast-landing.jpg)

## Haunted grounds

Snapshot [`75db377`] · [#14], promoted in [#15].

Three hours later, hero selection moved into a compact panel — “Your place in the story” — beside the haunted scene. A shambler patrols the verge, and each turn gained an explicit **Roll for movement** step.

![The compact Your place in the story panel with hero tiles beside the haunted house](../assets/zombies-history/06-haunted-grounds-setup.jpg)

## The living woods

Snapshot [`2b5cbea`] · [#16], [#19] and [#21], promoted in [#18], [#20] and [#22].

The house moved into the woods. Grass, forest floor, bark and clapboard textures were baked in Blender's Cycles renderer, and five procedural trees — three dead, two spruce — ring the clearing and hide whenever they would block the camera. The game board now matches the landing house. A pickup flyout shows what you found, and a pill under the title counts the rooms in the house and the games finished. The chimney smokes, the fireplace burns and monsters wander among the trees outside the fence. The turn dock became compact, with forgiving phone taps. This release was also the first whose checks ran on the owner's disposable Azure VMs.

<p>
  <img src="../assets/zombies-history/07-living-woods-landing.jpg" width="49%" alt="The house in dark foggy woods with the rooms and games finished pill under the title">
  <img src="../assets/zombies-history/07-living-woods-board.jpg" width="49%" alt="The board in a fenced clearing in the woods, with an objectives panel asking survivors to get back to the van">
</p>

## The getaway bus

Snapshot [`096a1f2`] · [#23], promoted in [#24].

A batch of play-testing notes. Setup shrank to one compact panel with a random scary table name, and the floating toolbar became a single **Menu** button. Furniture snapped to the walls. A hidden monster now stays underground until a survivor is one cell away, then rises out of the floor; defeated monsters topple and stay as ashen bodies. The van became an orange-and-white hippie getaway bus.

<p>
  <img src="../assets/zombies-history/08-getaway-bus-landing.jpg" width="49%" alt="Landing page with the orange and white hippie getaway bus parked by the house">
  <img src="../assets/zombies-history/08-getaway-bus-board.jpg" width="49%" alt="The board with a road leading in and a single Menu button in the top right corner">
</p>

## v16 — the faithful spinner

Snapshot [`ff88425`] · [#27], promoted in [#28].

The first release with a version in its name. The movement spinner now matches the original game, with weighted sectors in its 3:3:2:2 shares. **Autopilot** lets a bot take your seat until you take control again. A forest road with haunted gates and figurine posts leads to the van, an old corner hearth replaced the fireplace, and finished rooms sort last. Before any code was written, the plan was argued by two AI models, one challenging and one defending it.

![v16 board: a road through haunted gates to the van, the spinner dial showing 1 to 4, and the Autopilot switch in the turn dock](../assets/zombies-history/09-v16-board.jpg)

## v17 and v18 — cutscenes and the Hall of Fame

Snapshot [`ff6dda9`] (v17) · [#29], promoted in [#30]; v18 in [#31], promoted in [#32].

The game turned cinematic, with an arrival scene as the van pulls in and a departure as it escapes. The road became a swamp of puddles and ruts, the gate figurines became a detailed horned gargoyle and a clawed hooded ghoul with ember eyes, and an antique furniture kit arrived, with a car key that finally looks like a car key. Gameplay added the **Hall of Fame**, game timing and optional rescues, and the Flare Launcher became the Bazooka. That evening v18 fixed an arrival scene that did not play when a game started; our rebuild of v17 shows the same symptom. v18 also muddied the road, broke the antiques and added cutscene previews and an escape prompt.

![v17 landing page with gate figurines, a horned gargoyle and a hooded ghoul, along the road to the house](../assets/zombies-history/10-v17-landing.jpg)

## v19 — the van bay

Snapshot [`06749f5`] · [#33], promoted in [#34].

v19 went back to the original rules (Magellan, 2016): heroes may pass through each other but never stop on the same space, except on the truck at the finish. Instead of stacking, the finish got a real **van bay**, a 2×3 pad beside the van. Nobody shares a space anywhere, everyone starts in the bay, and the survivors standing in it escape together once they hold the key and the fuel. The house became a log cabin, the road sank into the ground and the arrival scene splashes mud. There were now four cutscenes.

![The v19 arrival cutscene in the dark woods with the caption Something in the trees is keeping pace](../assets/zombies-history/11-v19-arrival.jpg)

![v19 board with log-cabin walls and the dashed two-by-three van bay beside the van](../assets/zombies-history/11-v19-board.jpg)

## v20 to v23 — hearts, scores and discs

Snapshot [`1389669`] (v23) · [#38], [#40], [#42] and [#44], promoted in [#39], [#41], [#43] and [#45].

Four releases in 33 hours:

- **v20:** Board up, Break and Grenade became compact buttons beside **End turn**, and a rules check confirmed that the medkit has no health cap, as in the rulebook.
- **v21:** one heart per health point in the roster, a last-stand cutscene when the final survivor falls, and a haunted score with sound switches.
- **v22:** cutscene music matched to the mood, every seat moves on turn 1, and every hidden marker is visible.
- **v23:** a disc on every hidden cell (an audit found that furnished cells had none), a bazooka sling, barricades at any time, and a landing theme.

The landing page barely changed in these days; see the contact sheet above.

## v24 and v25 — the rulebook map

Snapshot [`0d1ae73`] (v25) · [#48] and [#50], promoted in [#49] and [#51].

v24 hid discs behind walls, held “found” news until the finder arrives and steadied **Take control**. v25 matched the yard and the encounter deck to the rulebook, replaced the road with Blender-baked gravel and a driveway that runs up to the bay, gave each soundtrack a clean reverb fade, and fitted piles of recovered items onto small screens. Browser tests split into a Node job and a Cloudflare job.

<p>
  <img src="../assets/zombies-history/13-v25-landing.jpg" width="49%" alt="v25 landing page with the road running up to the van and the house">
  <img src="../assets/zombies-history/13-v25-board.jpg" width="49%" alt="v25 board with a gravel road from the gates to the van bay and heart rows in the roster">
</p>

## v26 — smarter bots and a long polish

Snapshot [`84bd0de`] · [#52], [#56], [#60], [#64], [#66] and [#68], each promoted to `main`.

v26.0.0 taught bots to route around revealed monsters and to stand still rather than take a pointless step ([#52]). The rest of the week was play-testing polish under the same version number:

- The arrival is fully prepared before it plays, so the entrance no longer stutters. The same release brought a new many-eyed Warden and dates in the Hall of Fame ([#56]).
- You can board up only a door or window beside you ([issue #58]), and every fallen survivor gets their own scene ([issue #59]). Both shipped in [#60], together with a darker driveway.
- The table stays as you left it after a mid-game death scene ([#64]).
- The Hall of Fame shows the time, the Warden's reveal no longer freezes the game, walkers in the woods keep their feet on the ground and the van got rounder ([#66]).
- The landing camera tilts from nearly overhead down to eye level, and the van became an aerodynamic one-box ([#68]).

<p>
  <img src="../assets/zombies-history/14-v26-landing.jpg" width="49%" alt="v26 landing page with the aerodynamic one-box van and the redesigned red-eyed Warden">
  <img src="../assets/zombies-history/14-v26-arrival.jpg" width="49%" alt="The v26 arrival cutscene with the new van on the road">
</p>

## v27 — cheat codes

Snapshot [`06dba2a`] · [#70], promoted in [#71].

Old-school cheats arrived. Type `iddqd` and your survivor can no longer die: their hearts become a golden infinity sign modeled in Blender, and everyone at the table sees it. Type `idkfa` and you alone see what lies under every plate. Monsters and items appear as pale ghosts, neither discovered nor activated, so nobody playing for the undead can use them early. The Warden grew vampire wings. Survivors who reach the van get a checkmark beside their name, a bot that stopped one cell short of the bay now walks in, and on small screens the roster and the turn dock no longer hide each other.

<p>
  <img src="../assets/zombies-history/15-v27-landing.jpg" width="49%" alt="v27 landing page with the winged Warden beside the house">
  <img src="../assets/zombies-history/15-v27-idkfa.jpg" width="49%" alt="v27 board after IDDQD and IDKFA: undiscovered monsters and items as pale ghosts, supplies as flat slabs, and a golden infinity sign in place of hearts">
</p>

## v28 — today

Snapshot [`0504769`] · [#72], promoted in [#73].

Games played with a cheat no longer enter the Hall of Fame. The supplies were remodeled in Blender — an X-braced plank barricade, a jerry can and a van key with a tag — so they read at a glance, even as IDKFA ghosts; compare the flat slabs in v27 above. And the Warden finally walks: an armature lets it knuckle-walk on its arms with planted feet, in the woods, on the board and in cutscenes.

<p>
  <img src="../assets/zombies-history/16-v28-landing.jpg" width="49%" alt="v28 landing page with the winged Warden">
  <img src="../assets/zombies-history/16-v28-idkfa.jpg" width="49%" alt="v28 board after IDDQD and IDKFA: readable plank barricades, a jerry can and a key among the ghosts, and the winged Warden's ghost in the yard">
</p>

![The setup screen today: the Your place in the story panel with five Blender survivors, bot companions and rescued friends, beside the porch and the winged Warden](../assets/zombies-history/16-v28-setup.jpg)

## Behind the scenes

- After the first commit, every change went through a pull request to `dev`, then reached `main` in one of 31 promotion pull requests.
- After [#11], the suite had 34 unit tests, 16 integration tests and 12 browser journeys on each transport. With v28 it has 78 unit tests, 24 integration tests and 34 browser journeys, run both on Node and on the Cloudflare Worker.
- The game runs in production at <https://zombie-in-da-house.syntal.workers.dev>.

## How these pictures were made

Each snapshot is the `main` merge commit named in its chapter, rebuilt from scratch for this history:

```sh
git worktree add --detach /tmp/zidh-history/wt <commit>
cd /tmp/zidh-history/wt && npm ci && npm run build
NODE_ENV=test TEST_GAME_SEED=17 HOST=127.0.0.1 PORT=4290 \
  PUBLIC_APP_URL=http://127.0.0.1:4290 DATABASE_PATH=/tmp/zidh-history/<snapshot>.sqlite \
  node dist/server.js
```

- Every snapshot ran locally on Node with a fresh SQLite database and seed 17, so each build deals its own table reproducibly. The deck itself changed over time.
- Playwright drove Chromium with hardware WebGL (ANGLE on Metal, Apple silicon) at 1440×900. It opened the landing page and started a solo game with two bot companions. The first two snapshots predate solo play, so there it set a two-player shared-device table. It captured or skipped the arrival cutscene, then waited for the board.
- The cheat shots type `iddqd`, then `idkfa`, on the board and zoom in four steps.
- None of the 16 builds logged a console error.
- The later landing-page footers were cropped so rebuild timestamps are not mistaken for release dates.
- Images are 1200-pixel JPEGs; the contact sheet is a montage of the 16 landing pages.

[`f6e57cf`]: https://github.com/vasilyevstan/zombie-boardgame/commit/f6e57cfebff643989ff1c4c9a30b7eb1276b67c2
[`14b2572`]: https://github.com/vasilyevstan/zombie-boardgame/commit/14b25727de8e02e4ff196ad76dc7800dedfd129c
[`bb9056f`]: https://github.com/vasilyevstan/zombie-boardgame/commit/bb9056f1e3ad643fd4bc44a9235b5ab53260b091
[`48b47c9`]: https://github.com/vasilyevstan/zombie-boardgame/commit/48b47c91a9fc69770403902de21128e488eee0ec
[`7439b82`]: https://github.com/vasilyevstan/zombie-boardgame/commit/7439b82a5b0a042291b1ca72d13e8b159f50d103
[`75db377`]: https://github.com/vasilyevstan/zombie-boardgame/commit/75db3772d9aaf56abfeda72a380dbb4e051f5f33
[`2b5cbea`]: https://github.com/vasilyevstan/zombie-boardgame/commit/2b5cbeae401fa733c7ee5cf31fa8df24dbb322b9
[`096a1f2`]: https://github.com/vasilyevstan/zombie-boardgame/commit/096a1f2ff6f3484760676dcb33b7acf799827d64
[`ff88425`]: https://github.com/vasilyevstan/zombie-boardgame/commit/ff8842519b52339b42b52db8cac0ffc6313880af
[`ff6dda9`]: https://github.com/vasilyevstan/zombie-boardgame/commit/ff6dda906ea0b5b2fdfc529c730366be4def16cb
[`06749f5`]: https://github.com/vasilyevstan/zombie-boardgame/commit/06749f57c85de78fbf87f82bafcc1796b8535c18
[`1389669`]: https://github.com/vasilyevstan/zombie-boardgame/commit/1389669dee43e945e34c335b9c4b4b1241874080
[`0d1ae73`]: https://github.com/vasilyevstan/zombie-boardgame/commit/0d1ae73dda076a87a8f4a8aa89633f9fb8ee5aef
[`84bd0de`]: https://github.com/vasilyevstan/zombie-boardgame/commit/84bd0debf1e4707bf59cc28d0a178f7c54dc321a
[`06dba2a`]: https://github.com/vasilyevstan/zombie-boardgame/commit/06dba2a2078f1d884ae101612b63cb9ccb7b2aa8
[`0504769`]: https://github.com/vasilyevstan/zombie-boardgame/commit/05047697142be180f89a369ef6a999716209d4eb
[#1]: https://github.com/vasilyevstan/zombie-boardgame/pull/1
[#2]: https://github.com/vasilyevstan/zombie-boardgame/pull/2
[#3]: https://github.com/vasilyevstan/zombie-boardgame/pull/3
[#7]: https://github.com/vasilyevstan/zombie-boardgame/pull/7
[#8]: https://github.com/vasilyevstan/zombie-boardgame/pull/8
[#9]: https://github.com/vasilyevstan/zombie-boardgame/pull/9
[#10]: https://github.com/vasilyevstan/zombie-boardgame/pull/10
[#11]: https://github.com/vasilyevstan/zombie-boardgame/pull/11
[#12]: https://github.com/vasilyevstan/zombie-boardgame/pull/12
[#13]: https://github.com/vasilyevstan/zombie-boardgame/pull/13
[#14]: https://github.com/vasilyevstan/zombie-boardgame/pull/14
[#15]: https://github.com/vasilyevstan/zombie-boardgame/pull/15
[#16]: https://github.com/vasilyevstan/zombie-boardgame/pull/16
[#18]: https://github.com/vasilyevstan/zombie-boardgame/pull/18
[#19]: https://github.com/vasilyevstan/zombie-boardgame/pull/19
[#20]: https://github.com/vasilyevstan/zombie-boardgame/pull/20
[#21]: https://github.com/vasilyevstan/zombie-boardgame/pull/21
[#22]: https://github.com/vasilyevstan/zombie-boardgame/pull/22
[#23]: https://github.com/vasilyevstan/zombie-boardgame/pull/23
[#24]: https://github.com/vasilyevstan/zombie-boardgame/pull/24
[#27]: https://github.com/vasilyevstan/zombie-boardgame/pull/27
[#28]: https://github.com/vasilyevstan/zombie-boardgame/pull/28
[#29]: https://github.com/vasilyevstan/zombie-boardgame/pull/29
[#30]: https://github.com/vasilyevstan/zombie-boardgame/pull/30
[#31]: https://github.com/vasilyevstan/zombie-boardgame/pull/31
[#32]: https://github.com/vasilyevstan/zombie-boardgame/pull/32
[#33]: https://github.com/vasilyevstan/zombie-boardgame/pull/33
[#34]: https://github.com/vasilyevstan/zombie-boardgame/pull/34
[#38]: https://github.com/vasilyevstan/zombie-boardgame/pull/38
[#39]: https://github.com/vasilyevstan/zombie-boardgame/pull/39
[#40]: https://github.com/vasilyevstan/zombie-boardgame/pull/40
[#41]: https://github.com/vasilyevstan/zombie-boardgame/pull/41
[#42]: https://github.com/vasilyevstan/zombie-boardgame/pull/42
[#43]: https://github.com/vasilyevstan/zombie-boardgame/pull/43
[#44]: https://github.com/vasilyevstan/zombie-boardgame/pull/44
[#45]: https://github.com/vasilyevstan/zombie-boardgame/pull/45
[#48]: https://github.com/vasilyevstan/zombie-boardgame/pull/48
[#49]: https://github.com/vasilyevstan/zombie-boardgame/pull/49
[#50]: https://github.com/vasilyevstan/zombie-boardgame/pull/50
[#51]: https://github.com/vasilyevstan/zombie-boardgame/pull/51
[#52]: https://github.com/vasilyevstan/zombie-boardgame/pull/52
[#56]: https://github.com/vasilyevstan/zombie-boardgame/pull/56
[#60]: https://github.com/vasilyevstan/zombie-boardgame/pull/60
[#64]: https://github.com/vasilyevstan/zombie-boardgame/pull/64
[#66]: https://github.com/vasilyevstan/zombie-boardgame/pull/66
[#68]: https://github.com/vasilyevstan/zombie-boardgame/pull/68
[#70]: https://github.com/vasilyevstan/zombie-boardgame/pull/70
[#71]: https://github.com/vasilyevstan/zombie-boardgame/pull/71
[#72]: https://github.com/vasilyevstan/zombie-boardgame/pull/72
[#73]: https://github.com/vasilyevstan/zombie-boardgame/pull/73
[issue #58]: https://github.com/vasilyevstan/zombie-boardgame/issues/58
[issue #59]: https://github.com/vasilyevstan/zombie-boardgame/issues/59
