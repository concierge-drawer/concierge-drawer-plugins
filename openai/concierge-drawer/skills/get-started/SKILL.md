---
name: get-started
description: Run a hotel or restaurant's vouchers with Concierge Drawer. Use when the person asks how their vouchers or offers are doing, wants to create, publish or pause a voucher template, send a voucher to a guest, or invite and organize staff.
---

# Run vouchers with Concierge Drawer

Concierge Drawer replaces paper vouchers for hotels and restaurants. An organization (a hotel or restaurant group) owns businesses. Each business has voucher templates (its offers), which its staff send to guests by email or text. ChatGPT works with the person's own permissions, only in the organizations and businesses they chose when they connected. Organization members and business admins can connect.

## Find the business

Call `list_businesses` (or `list_organizations`). If there are several and the person hasn't said which, ask. Never invent an id. If the one they mean isn't listed, tell them to reconnect Concierge Drawer and select it.

## Results

Use `get_business_dashboard` or `get_organization_dashboard` with `range` `7d`, `30d` or `90d` for vouchers sent and redeemed, the redemption rate and its trend, top senders and top vouchers. Don't add up lists yourself.

## Offers

Draft the title, description, redemption instructions and terms with the person and show them before calling `create_voucher_template`. Save it as a draft unless they ask to publish it now. Publish, pause or resume with `update_voucher_template` and `status`.

## Sending a voucher

`send_voucher` emails or texts a real guest straight away. Show the person the offer and the guest's email or phone number and wait for a clear yes. Text a guest only when the person confirms the guest agreed to receive texts, and then pass `sms_consent: true`; otherwise send by email. Call it once, and check `list_sent_vouchers` before trying again after an error.

## Staff

Confirm the email and role before `invite_business_member` or `invite_organization_member`: the invitation is emailed right away. Confirm role changes, cancelled invitations and team removals first too. ChatGPT can't remove members, delete anything or change billing; those happen in Concierge Drawer's settings.
