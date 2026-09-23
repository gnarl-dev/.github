<img alt="Gnarl — decentralized search. The Gnarl knot above the wordmark, with Gnarly the St. Bernard below wearing a barrel marked with the same knot." src="https://raw.githubusercontent.com/gnarl-dev/.github/main/profile/assets/gnarl-stbernard-and-logo.png" width="680">

### Search data where it already lives.

Classical distributed search assumes the cheapest place to search your data is a
cluster you control, so step one is always the same: copy everything in. That
assumption is the thing that breaks.

- **Data gravity.** Petabyte archives cost more to move than to search.
- **Sovereignty.** Some data legally cannot cross a boundary, ever.
- **Blast radius.** One coordinator, one config change, one bad night.
- **Economics.** You pay twice — once to store it, once to duplicate it.

Gnarl turns each place your data already lives into a **node**. Nodes index
locally, publish a signed summary of what they can answer, and cooperate on
queries. The result is one searchable surface across many independent operators,
with no central cluster and nobody's data leaving home.

```bash
gnarl start                            # run a node on your own machine
gnarl mesh setup --mesh-name my-mesh   # create a private mesh, invite peers
gnarl search "satellite imagery"       # query everyone who can answer
```

## How a network is shaped

No coordinator. No elected master. No shared cluster state to keep in sync.
Three moving parts:

| Part | What it does |
| --- | --- |
| **Node** | Indexes a local corpus, answers queries against it, enforces its own policy |
| **Manifest** | A signed, compact statement of what a node *can* answer — never the documents |
| **Query plan** | Chooses which nodes to ask, fans out, merges the ranked results |

A query enters at *any* node. That node reads the manifests it knows about,
decides who is worth asking, fans out, and merges what comes back into one
ranked result set.

## Answers you can audit

A search that spans operators you do not control raises a question a single
cluster never has to answer: **was that all of it, and is any of it forged?**

Every response carries **coverage** — how many claims were expected, how many
answered, and why each missing one was skipped. Ask for `verify` and every
served claim must be *proven* against an anchor the node holds independently of
whoever served it.

A claim nobody could check counts against completeness exactly as an unanswered
one does. *Cannot check* is never reported as *checked* — those are different
states, and conflating them either invents trust or destroys good data.

## Clients

| Language | Repository | Install |
| --- | --- | --- |
| Go | [go-client](https://github.com/gnarl-dev/go-client) | `go get github.com/gnarl-dev/go-client` |
| Python | [python-client](https://github.com/gnarl-dev/python-client) | `pip install gnarl` |

Both are generated from the node's OpenAPI description, so their payload types
cannot drift from the server, and both run **conformance suites against a real
node** rather than only compiling. TypeScript and Java are next.

## About the dog

His name is Gnarly.

A St. Bernard finds what is buried. Not in a controlled environment and not with
precise coordinates — in bad weather, across terrain nobody mapped, working from
a faint signal and its own judgment. Then it brings back something useful.

That is the whole product description.

The barrel carries the Gnarl knot: an unbroken loop with no beginning and no
centre. That is the topology. There is no head node in a knot.

*(St. Bernards never actually carried brandy barrels — a painter added one in
1820 and it stuck. We kept it, because the useful part of the image is that the
dog arrives carrying something, not what is in the barrel.)*

---

[**Documentation**](https://gnarl.dev/docs) · [**Blog**](https://gnarl.dev/blog)
· [**Why a St. Bernard**](https://gnarl.dev/blog/why-a-st-bernard)

Built by [Lucenia](https://lucenia.io).
