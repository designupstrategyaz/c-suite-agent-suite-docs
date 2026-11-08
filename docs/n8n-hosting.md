# Where n8n runs

From the pack's `shared/n8n.md`.

All three workflows call n8n's **Execute Command** node. Whether that node exists depends on the host:

| Host | Execute Command | What to do |
|---|---|---|
| Self-hosted n8n before 2.0 | available | Import and run as above. |
| Self-hosted n8n 2.0 or later | **disabled by default** | The instance owner re-enables it (`NODES_EXCLUDE`, see n8n docs, "Block specific nodes"). It lets workflows run shell commands, so that is the owner's security call, not the agent's. |
| n8n Cloud | **not available** | Replace each Execute Command node with an SSH node (if the plan offers it) pointed at the machine that holds the pack, same `python3 scripts/…` command, working directory = pack root. Or skip n8n and run the same commands from cron or the buyer's agent. |
| No n8n | — | Nothing breaks. n8n is optional; every job runs by hand or by the agent. |

Never paste an SSH key or token into the workflow JSON; it stays in n8n's credential vault (`n8n.py --validate` fails on a raw key).

The inbound webhook workflow uses header auth and a Code node that rejects off-pattern fields and base64-encodes the rest, so no raw request text reaches the shell.
