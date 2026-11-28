# Use cases

What you can ask your agent once Concierge Drawer is connected. Each prompt below works in ChatGPT, Codex, Claude and Claude Code. The tool names show what the agent calls.

Concierge Drawer tells the agent to show you what it's about to create, send or change, and to wait for you to confirm.

## Setup

See the [README](README.md) for installing in your agent. In Codex CLI:

```bash
codex mcp add concierge-drawer --url https://mcp.conciergedrawer.com/mcp
codex mcp login concierge-drawer
```

Sign in at [conciergedrawer.com/auth](https://conciergedrawer.com/auth) and choose the organizations and businesses the agent can use.

## Walkthrough: launch an offer and send it to a guest

1. **Find the business**

   > Which businesses can you work in?

   Calls `list_organizations` and `list_businesses`.

2. **Check the numbers**

   > How did our vouchers do this month?

   Calls `get_business_dashboard`: vouchers sent and redeemed, and the redemption rate.

3. **Draft an offer**

   > Create a voucher for a complimentary welcome drink at the bar, good for 30 days. Show me the wording before you create it.

   The agent drafts the title, description, redemption instructions and terms, then waits for you.

4. **Publish it**

   > Looks good, create it and publish it.

   Calls `create_voucher_template`. An active template can be sent by staff right away.

5. **Send it to a guest**

   > Send it to guest@example.com with the note "Welcome back, the first round is on us."

   The agent repeats the template, address and note back to you.

6. **Confirm the send**

   > Yes, send it.

   Calls `send_voucher`. The guest gets an email with a link to their voucher and its 6-digit code.

7. **See the results**

   > Who have we sent the welcome drink to?

   Calls `list_sent_vouchers`: who it went to, who sent it, and whether it was redeemed.

When the offer ends:

> Pause the welcome drink template.

Calls `update_voucher_template`. Staff can no longer send it.

## More things to ask

### Results

> Which offers do guests actually redeem?

> Compare this month's redemptions with last month across all our hotels.

Calls `list_voucher_templates`, `get_business_dashboard` and `get_organization_dashboard`.

### Offers

> Make the spa credit available to our partner hotels.

> Change the dessert voucher so it expires after 14 days.

Calls `get_voucher_template` and `update_voucher_template`.

### Sending

> Text the late checkout voucher to +1 512 555 0100.

The agent asks whether the guest agreed to receive texts, and offers email if not. Calls `send_voucher`.

### Staff and teams

> Invite jamie@example.com to the hotel as a member.

> Create a Concierge team with Alex and Sam.

> Who sent the most vouchers last week?

Calls `invite_business_member`, `create_business_team` and `list_business_members`.

### Activity

> Who changed the welcome drink template, and when?

Calls `get_audit_log`.

## What the agent won't do

- **Text a guest without consent.** It asks first, and offers email instead.
- **Delete things.** Businesses, organizations and voucher templates are deleted in Concierge Drawer itself. Pause a template instead.
- **Change billing.** Plans and payment live on the Billing page in Concierge Drawer.
