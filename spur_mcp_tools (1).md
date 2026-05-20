# Spur MCP Tools — Complete Reference
> All 42 tools available via the Spur MCP integration on spurnow.com, segmented by category.

---

## 1. Channels

| Tool | Description |
|------|-------------|
| `channels_list` | List all workspace channels and sender handles by type (WhatsApp, Instagram, Facebook, Live Chat) |

---

## 2. Contacts

| Tool | Description |
|------|-------------|
| `contact_search` | Search contacts in the workspace using filters like name, phone, email, or tags |
| `contact_create` | Create a new contact with phone, name, tags, and custom fields |
| `contact_update` | Update an existing contact — partial patch supported (name, tags, custom fields) |
| `contact_delete` | Delete a contact by contact ID or WhatsApp phone number |
| `contact_tag_search` | Search workspace contact tags by name to discover tag IDs before building segments or assigning tags |

---

## 3. Contact Imports (CSV)

| Tool | Description |
|------|-------------|
| `customer_import_csv_validate` | Validate a contact-import CSV using the same backend flow as the Spur UI import — checks column mapping and data format before starting |
| `customer_import_csv_start` | Start a background CSV contact import with tag assignment using the same queue pipeline as the Spur UI |
| `customer_import_csv_status` | Fetch current CSV import progress — rows processed, errors, completion status |
| `customer_import_csv_clear` | Clear completed or failed import job status for the workspace |

---

## 4. Segments

| Tool | Description |
|------|-------------|
| `segment_filters_catalog` | Fetch all available segment filter categories, fields, and operators to build valid segment rule groups |
| `segment_create` | Create a SPUR segment from a complete ruleGroup payload |
| `segment_update` | Update an existing SPUR segment's rules or name |
| `segment_delete` | Delete a segment by ID |
| `segment_search` | Search segments with optional type filter (SPUR or SHOPIFY) and cursor pagination |
| `segment_validate` | Validate a SPUR segment ruleGroup without saving — normalises tag names to IDs and date strings |
| `segment_size` | Fetch audience size for an existing segment and return whether it is currently valid for broadcast |

---

## 5. Shopify Segments

| Tool | Description |
|------|-------------|
| `shopify_segment_sync` | Sync all Shopify customer segments from the connected store into Spur local segments |
| `shopify_segment_create` | Create a native Shopify customer segment using ShopifyQL query syntax |
| `shopify_segment_update` | Update a native Shopify customer segment by Shopify segment ID |
| `shopify_segment_delete` | Delete a native Shopify customer segment by Shopify segment ID (numeric or GID) |
| `shopify_segment_validate` | Validate a Shopify segment query before create or update |
| `shopify_segment_query_reference` | Return guidance for when to use Shopify vs Spur segments, plus ShopifyQL query syntax reference |

---

## 6. Templates

| Tool | Description |
|------|-------------|
| `template_search` | Search WhatsApp templates in the workspace with optional status, category, and channel filters |
| `template_create_draft` | Create a WhatsApp template draft in Spur without submitting it to Meta |
| `template_update_draft` | Update an existing draft template by internal template ID (copy, buttons, media) |
| `template_submit_for_review` | Submit an existing draft template for Meta review and return detailed provider error messages if rejected |
| `template_refresh` | Refresh a template's approval status from Meta using internal template ID or metaTemplateId |
| `template_requirements` | Inspect a template and return all runtime parameter requirements (variables, media, button URLs) plus a safe sample payload |
| `template_delete` | Delete a template by internal Spur template ID |

---

## 7. Media Uploads

| Tool | Description |
|------|-------------|
| `whatsapp_media_upload` | Upload runtime media from a URL for a workspace WhatsApp channel — returns a media ID for use in message_send |
| `whatsapp_template_media_upload` | Upload media from a URL via Meta resumable uploads and return a header_handle for use in template header components |

---

## 8. Messaging (1:1)

| Tool | Description |
|------|-------------|
| `message_send` | Send a message to a single WhatsApp, Instagram, or Facebook contact using an existing SPUR template or free-form text |

---

## 9. Conversations

| Tool | Description |
|------|-------------|
| `conversation_search` | Search and list conversations in the workspace — supports filtering by channel, unread status, contact name |
| `conversation_messages` | Fetch messages in a conversation ordered by most recent first — supports ISO cursor pagination |

---

## 10. Broadcasts

| Tool | Description |
|------|-------------|
| `broadcast_search` | Search broadcasts for ID discovery, status checks, and pagination |
| `broadcast_create` | Create a draft broadcast with a template reference, segment targeting, optional variable payload, and scheduling options |
| `broadcast_update` | Update a DRAFT broadcast — change template variables, include/exclude segment operations, sendAt datetime, deliveryBoost, splitAcrossDays |
| `broadcast_launch` | Launch a broadcast by calling setReady — transitions the broadcast from DRAFT to active send |
| `broadcast_analytics` | Fetch per-broadcast analytics including delivery, open, click, reply metrics, revenue, orders, and WhatsApp cost |
| `broadcast_overview_stats` | Fetch workspace-level broadcast overview KPIs — total count, total revenue, and aggregate engagement across all campaigns |
| `broadcast_recipients` | Fetch the per-recipient delivery breakdown for a sent broadcast — filterable by status (sent, delivered, opened, clicked, failed) |

---

## Quick Reference — Tools by Use Case

| Use Case | Primary Tools |
|----------|--------------|
| Send bulk campaign | `template_search` → `segment_search` → `broadcast_create` → `broadcast_launch` → `broadcast_analytics` |
| 1:1 transactional message | `channels_list` → `template_requirements` → `message_send` |
| Import & target new contacts | `customer_import_csv_validate` → `customer_import_csv_start` → `contact_tag_search` → `segment_create` → `broadcast_create` |
| Create & approve template | `template_create_draft` → `whatsapp_template_media_upload` → `template_submit_for_review` → `template_refresh` |
| Shopify campaign | `shopify_segment_sync` → `segment_size` → `broadcast_create` → `broadcast_launch` |
| Analyse campaign performance | `broadcast_search` → `broadcast_analytics` → `broadcast_recipients` → `broadcast_overview_stats` |
| Manage inbox | `conversation_search` → `conversation_messages` → `message_send` |
| Build audience segment | `segment_filters_catalog` → `segment_validate` → `segment_create` → `segment_size` |

---

*Source: Spur MCP integration — spurnow.com*

---

# Spur MCP — Problem-to-Tool Combination Guide
> Step-by-step tool chains for every problem type you can solve with Spur MCP.

---

## Problem 1 — Send a Bulk Promotional Broadcast
**Category:** Marketing
**Objective:** Reach thousands of contacts with a single campaign.

| Step | Action | Tool | Optional? |
|------|--------|------|-----------|
| 1 | Discover approved templates | `template_search` | No |
| 2 | Check template variable requirements | `template_requirements` | No |
| 3 | Find or create your target segment | `segment_search` / `segment_create` | No |
| 4 | Validate segment audience size | `segment_validate` + `segment_size` | No |
| 5 | Create broadcast draft with segment + template | `broadcast_create` | No |
| 6 | Adjust scheduling or delivery settings | `broadcast_update` | Yes |
| 7 | Launch the broadcast | `broadcast_launch` | No |
| 8 | Pull delivery + engagement metrics | `broadcast_analytics` | No |

> **Key note:** For large lists — import CSV first → tag the cohort → build a SPUR_TAGS segment → broadcast. Never loop `message_send` for bulk sends.

---

## Problem 2 — Cart Abandonment or Re-engagement Campaign
**Category:** Marketing
**Objective:** Win back lapsed contacts or people who didn't complete a purchase.

| Step | Action | Tool | Optional? |
|------|--------|------|-----------|
| 1 | Get available filter fields | `segment_filters_catalog` | No |
| 2 | Build segment: last active > N days or Shopify cart abandoned | `segment_create` | No |
| 3 | Validate segment before use | `segment_validate` + `segment_size` | No |
| 4 | Find re-engagement template | `template_search` | No |
| 5 | Create broadcast, plan exclusions | `broadcast_create` | No |
| 6 | Exclude a recently engaged segment | `broadcast_update` (excludeAdd) | Yes |
| 7 | Launch | `broadcast_launch` | No |
| 8 | Track who clicked vs didn't | `broadcast_recipients` (status=clicked) | No |

> **Key note:** Use segment exclusion (`excludedSegmentIds`) to suppress already-engaged contacts. Combine SPUR activity filters with Shopify last order date for precision.

---

## Problem 3 — Event-Triggered or Time-Sensitive Blast
**Category:** Marketing
**Objective:** Flash sales, limited-time offers, or event announcements at a precise time.

| Step | Action | Tool | Optional? |
|------|--------|------|-----------|
| 1 | Create or find LTO/promo template | `template_search` / `template_create_draft` | No |
| 2 | Submit for Meta review if new | `template_submit_for_review` | No |
| 3 | Monitor approval status | `template_refresh` | No |
| 4 | Create broadcast with target segment | `broadcast_create` | No |
| 5 | Set sendAt ISO datetime for scheduled delivery | `broadcast_update` (sendAt) | No |
| 6 | Enable delivery boost for faster send | `broadcast_update` (deliveryBoost) | Yes |
| 7 | Launch | `broadcast_launch` | No |
| 8 | Pull live analytics during the window | `broadcast_analytics` | No |

> **Key note:** `deliveryBoost` and `splitAcrossDays` are mutually exclusive. For flash sales use `deliveryBoost`. For large audiences over multiple days, use `splitAcrossDays`.

---

## Problem 4 — Order Confirmation or Transactional Alert
**Category:** Operations
**Objective:** Send a utility message to a single contact — order update, delivery alert, OTP.

| Step | Action | Tool | Optional? |
|------|--------|------|-----------|
| 1 | List available channels | `channels_list` | No |
| 2 | Find the right UTILITY template | `template_search` (category=UTILITY) | No |
| 3 | Inspect required variables | `template_requirements` | No |
| 4 | Send directly to contact's number | `message_send` | No |

> **Key note:** Use `message_send` for 1:1 transactional sends only. UTILITY templates are cheaper than MARKETING. Never use `message_send` in a loop for bulk — use broadcast instead.

---

## Problem 5 — Review and Respond to Customer Conversations
**Category:** Support
**Objective:** Read inboxes, check unread messages, and follow up with specific contacts.

| Step | Action | Tool | Optional? |
|------|--------|------|-----------|
| 1 | Search conversations (unread or by contact name) | `conversation_search` | No |
| 2 | Fetch message thread for a specific conversation | `conversation_messages` | No |
| 3 | Look up contact details for context | `contact_search` | Yes |
| 4 | Send a reply message | `message_send` | No |

> **Key note:** Use `unreadOnly: true` to surface priority inboxes. `conversation_messages` paginates via ISO cursor. Combine with `contact_search` to pull the contact's full profile before replying.

---

## Problem 6 — Import a New List of Contacts from CSV
**Category:** Data
**Objective:** Onboard a new cohort from a file, tag them, then target with a broadcast.

| Step | Action | Tool | Optional? |
|------|--------|------|-----------|
| 1 | Validate the CSV mapping before importing | `customer_import_csv_validate` | No |
| 2 | Start the background import with tags | `customer_import_csv_start` | No |
| 3 | Poll import progress | `customer_import_csv_status` | No |
| 4 | Clear status once complete | `customer_import_csv_clear` | Yes |
| 5 | Find the import tag ID | `contact_tag_search` | No |
| 6 | Build a SPUR_TAGS segment from the import tag | `segment_create` | No |
| 7 | Verify segment audience size | `segment_size` | No |
| 8 | Launch broadcast to the new cohort | `broadcast_create` + `broadcast_launch` | No |

> **Key note:** Always validate CSV before starting. After import, use SPUR_TAGS segment (not phone lists) for broadcasts — it scales to any cohort size.

---

## Problem 7 — Manage Individual Contacts (CRUD)
**Category:** Contacts
**Objective:** Create, update, tag, or delete a single contact record.

| Step | Action | Tool | Optional? |
|------|--------|------|-----------|
| 1 | Search for an existing contact | `contact_search` | No |
| 2 | Create a new contact if not found | `contact_create` | No |
| 3 | Update name, phone, tags, or custom fields | `contact_update` | No |
| 4 | Find tag IDs to assign | `contact_tag_search` | Yes |
| 5 | Delete a contact if needed | `contact_delete` | Yes |

> **Key note:** `contact_update` supports partial patches. Use `contact_tag_search` first to get the exact tag ID before assigning. Deletion by phone number is supported as an alternative to contact ID.

---

## Problem 8 — Build and Maintain Audience Segments
**Category:** Segments
**Objective:** Create precise targeting rules, validate them, update or clean up old segments.

| Step | Action | Tool | Optional? |
|------|--------|------|-----------|
| 1 | Explore available filter fields and operators | `segment_filters_catalog` | No |
| 2 | Draft and validate your rule group | `segment_validate` | No |
| 3 | Create the segment | `segment_create` | No |
| 4 | Check audience size before use | `segment_size` | No |
| 5 | Update rules as criteria evolve | `segment_update` | Yes |
| 6 | Search existing segments to avoid duplicates | `segment_search` | No |
| 7 | Delete stale or unused segments | `segment_delete` | Yes |

> **Key note:** Always validate before saving — `segment_validate` normalises tag names to IDs and date strings. Never launch a broadcast with `segment_size = 0`.

---

## Problem 9 — Create and Get a WhatsApp Template Approved
**Category:** Templates
**Objective:** Draft a new template, iterate, submit for Meta review, and track status.

| Step | Action | Tool | Optional? |
|------|--------|------|-----------|
| 1 | List channels to get channelId | `channels_list` | No |
| 2 | Create draft with category + components | `template_create_draft` | No |
| 3 | Revise copy, buttons, or media | `template_update_draft` | Yes |
| 4 | Upload media header if needed | `whatsapp_template_media_upload` | Yes |
| 5 | Submit to Meta for review | `template_submit_for_review` | No |
| 6 | Sync approval status from Meta | `template_refresh` | No |
| 7 | Inspect runtime variables before first send | `template_requirements` | No |
| 8 | Delete rejected templates that won't be re-used | `template_delete` | Yes |

> **Key note:** MARKETING is the catch-all category. UTILITY requires strictly non-promotional content. Repeated misclassification triggers Meta restrictions. Always run `template_requirements` before your first broadcast.

---

## Problem 10 — Measure Broadcast Performance and Diagnose Failures
**Category:** Analytics
**Objective:** Pull campaign KPIs, drill into recipients by status, and compare across broadcasts.

| Step | Action | Tool | Optional? |
|------|--------|------|-----------|
| 1 | Find the broadcast by name | `broadcast_search` | No |
| 2 | Pull high-level delivery + engagement metrics | `broadcast_analytics` | No |
| 3 | Get workspace-level KPI overview | `broadcast_overview_stats` | No |
| 4 | List recipients filtered by status | `broadcast_recipients` (status=failed/clicked/opened) | No |
| 5 | Look up a specific contact from the failed list | `contact_search` | Yes |

> **Key note:** `broadcast_recipients` status precedence: clicked > opened > delivered > failed > sent. For failed contacts, combine with `contact_search` to check if the number is invalid.

---

## Problem 11 — Use Shopify Segments for Targeted Campaigns
**Category:** Shopify
**Objective:** Sync Shopify customer segments and use them directly in Spur broadcasts.

| Step | Action | Tool | Optional? |
|------|--------|------|-----------|
| 1 | Sync Shopify segments into Spur | `shopify_segment_sync` | No |
| 2 | Check available Shopify segments | `segment_search` (type=SHOPIFY) | No |
| 3 | Understand when to use Shopify vs Spur segments | `shopify_segment_query_reference` | Yes |
| 4 | Create a native Shopify segment if needed | `shopify_segment_create` | Yes |
| 5 | Validate audience size | `segment_size` | No |
| 6 | Add Shopify segment to broadcast | `broadcast_create` (includedSegmentIds only) | No |
| 7 | Launch and track | `broadcast_launch` + `broadcast_analytics` | No |

> **Key note:** Shopify segments can only be used in `includedSegmentIds` — not in `excludedSegmentIds`. Always sync before using to ensure the segment reflects current Shopify data.

---

*Source: Spur MCP integration — spurnow.com*
