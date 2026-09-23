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

## Four moving parts, none of them central

No coordinator. No elected master. No shared cluster state to keep in sync.

| Part | What it does |
| --- | --- |
| **Node** | One process, one identity, one or more local indexes. The only component that touches data. |
| **Manifest** | The signed summary a node advertises: fields, ranges, footprints, vocabulary. Never documents. |
| **Planner** | Eliminates nodes that cannot contribute, ranks the survivors, fans out within a budget. |
| **Verifier** | Checks every result signature against known identities before a hit reaches the caller. |

A query enters at *any* node. That node reads the manifests it knows about,
eliminates everyone who provably cannot help, fans out to the rest with a
deadline attached, and merges what comes back into one ranked result set.

Planning is local and takes microseconds. The wall clock is dominated by the
slowest node you decided to wait for — which is why the deadline is yours.

## What a node gives you

- **Coordinator-free fan-out.** Any node can plan a query. The one that receives
  it coordinates that query and nothing else — there is no master to elect,
  lose, or fail over.
- **Data never moves.** Corpora are opened read-only and indexed in place. The
  query travels to the data; the documents stay on the host that owns them.
- **Signed, verifiable results.** Every hit carries the signature of the node
  that produced it, not the node that merged it. Tampering in transit is
  detectable, and detected by default.
- **Policy at the edge.** Each node decides who may ask, which indexes are
  visible, which fields come back, and how fast — evaluated locally, every time,
  before the index is touched.
- **Text, geo, and vector.** Full-text, keyword, range, geospatial containment
  and kNN vector search, composed with boolean clauses in one query.
- **Peers of every size.** The same node runs on a server rack, a laptop, or a
  phone in the field. Cloud to edge, one protocol, no privileged tier.

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

`--explain` shows you the whole thing, because the node already knew it and used
to throw it away at the HTTP boundary:

```
$ gnarl search "king tide" --index places --explain

▸ plan       1 index(es) resolved on this node
▸ fan-out    41 claim(s) · 11/12 node(s) answered · deadline 750ms
   ├─ n1qh7f…c2a8   ✓ 18 claim(s)  remote_replica
   ├─ n1m4kd…91b3   ✓ 14 claim(s)  remote_replica
   ├─ e22b5b…5358   ✓  3 claim(s)  local_primary
   ├─ claim 37             ✗ timeout
   └─ claim 40             ✗ unreachable
▸ merge      124 hit(s) ranked
▸ verify     32 proven · 2 unverifiable · 0 failed · 3 local
   coverage  39/41 claims answered · complete: false
   took      128ms
```

Four verification states, not two, and they account for every claim. A search
that cannot tell you which peers answered is asking to be trusted rather than
audited.

## Built for data that will not move

- **Federated geospatial archives.** Imagery too large to centralize stays with
  the operator that collected it, while analysts query the whole constellation
  as one surface.
- **Sovereign and regulated data.** Residency stops being an architecture
  problem when documents never cross a boundary in order to be searched.
- **Edge and disconnected operations.** Nodes keep answering locally when the
  uplink drops, and reconcile manifests when it returns.
- **Multi-cloud without egress.** Three clouds, three nodes, one query. Stop
  paying to funnel everything into a fourth place.

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
