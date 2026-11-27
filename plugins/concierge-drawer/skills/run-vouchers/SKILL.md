---
name: run-vouchers
description: Run a hotel or restaurant's vouchers with Concierge Drawer. Use when the person asks how their vouchers or offers are doing, wants to create, edit, publish or pause a voucher template, send a voucher to a guest, see who sent or redeemed vouchers, or invite and organize staff.
---

# Run vouchers with Concierge Drawer

This plugin adds Concierge Drawer's MCP server, `concierge-drawer`. Concierge Drawer replaces paper vouchers for hotels and restaurants:

- An **organization** is the company (a hotel group, a restaurant group). It owns one or more **businesses**: a hotel, a restaurant, a bar, a spa.
- A business has **voucher templates**: its offers, like a complimentary dessert or a welcome drink. A template is `draft`, `active` (staff can send it) or `paused`.
- The business's **staff** (concierges, front desk) send **vouchers** from a template to guests by email or text, and staff redeem them, sometimes with a 6-digit code.

Every tool needs the person signed in. The first call asks your app to sign them in, and they pick which organizations and businesses it can use. Organization members and business admins can connect. The connector works with the person's own permissions: when a tool says they don't have permission, tell them and suggest asking an organization admin. Don't retry.

## 1. Find the business

Call `list_organizations` or `list_businesses`. If there are several and the person hasn't said which, ask. Never guess, and never invent an id: use the ids the tools return. If the one they mean isn't listed, tell them to reconnect Concierge Drawer and select it.

## 2. Answer questions with the dashboard

For numbers (vouchers sent and redeemed, redemption rate, trends, top senders, top vouchers) call `get_business_dashboard` or `get_organization_dashboard` with `range` `7d`, `30d` or `90d`. Don't add up lists yourself. For one template's sends, use `list_sent_vouchers`; for one staff member's, `list_business_member_vouchers`.

## 3. Create or change an offer

1. Draft the `title` (up to 64 characters), `description`, `redemption_instructions` and `terms` with the person, plus anything else they care about: `offer_value`, `expires_in_days`, `per_stay_limit`, `require_code_confirmation`, `visibility`.
2. Show them the draft and wait for their yes.
3. Call `create_voucher_template`. Leave `status` out to save it as a draft. Pass `status: "active"` only when they ask to publish it now: an active template can be sent by staff right away, and a `public` one by any concierge on Concierge Drawer.
4. To publish, pause or resume later, call `update_voucher_template` with only `status`. To edit, pass only the fields that change. Vouchers already sent keep their wording.

`visibility` is `private` (this business's staff), `partner` or `public`. `distribution_mode` `restricted` limits sending to the teams in `team_ids` (from `list_business_teams`).

## 4. Send a voucher to a guest

`send_voucher` sends a real email or text straight away.

1. Find the active template with `list_voucher_templates`.
2. Show the person the offer, the guest's email or phone number, any `note`, and `redemption_limit` if it's more than 1. Wait for a clear yes.
3. **Texts need the guest's consent.** Pass `recipient_phone` with `sms_consent: true` only when the person confirms the guest agreed to receive texts. If they haven't, send by email, or ask them to check with the guest first. Never assume consent.
4. Call `send_voucher` once. If it fails or times out, check `list_sent_vouchers` before trying again, so the guest doesn't get it twice.

## 5. Staff and teams

- To invite someone, ask whether they join one business (`invite_business_member`) or the whole organization (`invite_organization_member`), and which role: `member` for concierges and front desk, `admin` for managers. Confirm the email and role, then call the tool once: it emails the invitation right away.
- To change a role, cancel an invitation or remove someone from a team, confirm with the person first. These take effect at once.
- The connector can't remove members, delete teams, templates or businesses, or change billing. Point the person to Concierge Drawer's settings for those.

## Privacy

Guest emails and phone numbers are personal data. Use them only to send the voucher the person asked for, and don't repeat them back beyond what's needed to confirm.
