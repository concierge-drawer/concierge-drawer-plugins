<img src="assets/icon.svg" alt="Concierge Drawer" width="96" height="96">

# Concierge Drawer plugins

Run your hotel or restaurant vouchers from Claude Code, Codex or GitHub Copilot. The plugin adds Concierge Drawer's MCP server and a `run-vouchers` skill: your agent checks how your offers perform, drafts and publishes voucher templates, sends vouchers to guests once you confirm who they go to, and invites and organizes your staff. It works with your own permissions, only in the organizations and businesses you choose when you connect. Organization members and business admins can connect.

## Install

Claude Code:

```sh
claude plugin marketplace add concierge-drawer/concierge-drawer-plugins
claude plugin install concierge-drawer@concierge-drawer
```

Codex:

```sh
codex plugin marketplace add concierge-drawer/concierge-drawer-plugins
```

Then open `/plugins` in Codex, switch to the Concierge Drawer marketplace and install Concierge Drawer. Start a new session to use it.

GitHub Copilot CLI:

```sh
copilot plugin marketplace add concierge-drawer/concierge-drawer-plugins
copilot plugin install concierge-drawer@concierge-drawer
```

VS Code finds plugins you install with Copilot CLI. Without the CLI, run **Chat: Open Customizations**, add the `concierge-drawer/concierge-drawer-plugins` marketplace under **Browse → Configure Marketplaces**, and install Concierge Drawer.

Then ask, for example: "How did our vouchers do this week?"

## Only the connector

Claude, ChatGPT and other MCP clients can connect directly, without the skill, at `https://mcp.conciergedrawer.com/mcp`. Setup steps for each app are on [conciergedrawer.com/agents](https://conciergedrawer.com/agents).

## What's inside

| Path | What it is |
| --- | --- |
| `plugins/concierge-drawer/skills/run-vouchers/SKILL.md` | How the agent finds the business, reads results, creates offers, sends vouchers and invites staff |
| `plugins/concierge-drawer/.mcp.json` | The MCP server, for Claude Code |
| `plugins/concierge-drawer/mcp.json`, `plugin.json` | The same plugin in the Agent Plugins format, for Codex and GitHub Copilot |
| `openai/concierge-drawer/` | The ChatGPT app: its listing, OAuth settings, review test cases and `get-started` skill |
| `.claude-plugin/marketplace.json` | The Claude Code and GitHub Copilot marketplace |
| `.agents/plugins/marketplace.json` | The Codex marketplace |
| `registry/server.json` | The server's entry in the [MCP Registry](https://registry.modelcontextprotocol.io) |

## Tools

| Tool | What it does |
| --- | --- |
| `get_account` | Who is signed in, and the permissions they gave the app |
| `list_organizations`, `get_organization` | The organizations the app can use, with the person's role |
| `list_businesses`, `get_business` | The hotels, restaurants and other venues the app can use |
| `get_organization_dashboard`, `get_business_dashboard` | Vouchers sent and redeemed, redemption rate, trend, top senders and top vouchers |
| `get_audit_log` | Who did what in an organization. Organization admins only |
| `list_voucher_templates`, `get_voucher_template` | A business's offers and their results |
| `create_voucher_template`, `update_voucher_template` | Creates, edits, publishes, pauses and resumes an offer |
| `list_sent_vouchers` | Vouchers sent from an offer: recipient, sender, redeemed or expired |
| `send_voucher` | Sends a guest a voucher by email, or by text with the guest's consent |
| `list_organization_members`, `list_business_members`, `get_business_member` | Members and staff, with roles and sending stats |
| `list_business_member_vouchers` | The vouchers one staff member sent |
| `update_organization_member_role`, `update_business_member_role` | Changes a role |
| `list_organization_invitations`, `list_business_invitations` | Pending invitations |
| `invite_organization_member`, `invite_business_member` | Emails an invitation |
| `cancel_organization_invitation`, `cancel_business_invitation` | Cancels an invitation |
| `list_organization_teams`, `get_organization_team`, `list_business_teams`, `get_business_team` | Teams and how many members each has |
| `list_organization_team_members`, `list_business_team_members` | Who is on a team |
| `create_organization_team`, `update_organization_team`, `create_business_team`, `update_business_team` | Creates and renames teams |
| `add_organization_team_member`, `remove_organization_team_member`, `add_business_team_member`, `remove_business_team_member` | Adds and removes team members |

Prompts: `send_voucher`, `weekly_report`, `invite_staff`.

Nothing reaches a guest or a new member without your say-so: the agent confirms every voucher and invitation before sending it. Texting a guest needs their consent, and the connector refuses a phone number unless the agent passes `sms_consent`, which it does only once you confirm the guest agreed. The connector can't change billing or delete anything: organizations, businesses, voucher templates, teams or members.

You see the apps you connected under **Account → Connected apps** in Concierge Drawer, and can disconnect one there. Admins see every app connected to their organization or business, with who connected it, under **Settings → Connected apps**, and can remove its access.

## Privacy and support

- Privacy policy: [conciergedrawer.com/legal/privacy](https://conciergedrawer.com/legal/privacy)
- Terms: [conciergedrawer.com/legal/terms](https://conciergedrawer.com/legal/terms)
- Support: [support@conciergedrawer.com](mailto:support@conciergedrawer.com)

## Publishing

Bump `version` in the three `plugin.json` files when the plugin changes.

The server is listed in the MCP Registry as `com.conciergedrawer/concierge-drawer`. Bump `version` in `registry/server.json` with the server's, then publish it. The `com.conciergedrawer/*` names are proven with a DNS TXT record on `conciergedrawer.com` ([how](https://modelcontextprotocol.io/registry/authentication.md#dns-authentication)):

```sh
mcp-publisher login dns --domain conciergedrawer.com --private-key <key>
(cd registry && mcp-publisher publish)
```

To make the key, and the `v=MCPv1` TXT record to add on `conciergedrawer.com`:

```sh
openssl genpkey -algorithm Ed25519 -out key.pem
echo "v=MCPv1; k=ed25519; p=$(openssl pkey -in key.pem -pubout -outform DER | tail -c 32 | base64)"  # the TXT record
openssl pkey -in key.pem -outform DER | tail -c 32 | xxd -p -c 64                                  # the hex key for login
```

## License

MIT
