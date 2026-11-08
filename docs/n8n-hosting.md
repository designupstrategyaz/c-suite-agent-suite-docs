# Where n8n runs

From the pack's `shared/n8n.md`.

The three base workflows call n8n's **Execute Command** node; the three `*-ssh.json` versions call the **SSH** node instead and chain the kill check with `&&`, so the job runs only when the check exits 0. Which to import depends on the host:

| Host | Execute Command | What to do |
|---|---|---|
| Self-hosted n8n before 2.0 | available | Import and run as above. |
| Self-hosted n8n 2.0 or later | **disabled by default** | Import the `*-ssh.json` workflows, or the instance owner re-enables Execute Command (`NODES_EXCLUDE`, see n8n docs, "Block specific nodes"). Re-enabling lets workflows run shell commands, so that is the owner's security call, not the agent's. |
| n8n Cloud | **not available** | Import the `*-ssh.json` workflows. They run the same allowlisted commands through n8n's SSH node on the machine that holds the unzipped pack (Working Directory = the pack folder; SSH key in n8n's vault). Check that your plan lists the SSH node. Or skip n8n and run the same commands from cron or the buyer's agent. |
| No n8n | — | Nothing breaks. n8n is optional; every job runs by hand or by the agent. |

Never paste an SSH key or token into the workflow JSON; it stays in n8n's credential vault (`n8n.py --validate` fails on a raw key).

The inbound webhook workflow uses header auth and a Code node that rejects off-pattern fields and base64-encodes the rest, so no raw request text reaches the shell.
