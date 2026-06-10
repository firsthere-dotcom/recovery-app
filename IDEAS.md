# Ideas & future directions

Notes on what it would take to grow this app beyond a personal tool.
None of these are urgent — it works well as-is for a single user.

---

## Opening it up to other people

As currently set up, **anyone with a GitHub account can already sign in** and
get their own private data space (row-level security keeps every user's entries
completely separate). The barriers to making it genuinely accessible to others:

### 1. Friendlier login
GitHub-only is a wall for non-developers. Most people don't have GitHub.
Options (Supabase supports all of these):
- Email + password
- Magic link (email)
- Google
- Apple (required if distributing via iOS)

### 2. User-editable Exercise 5 (Seven Sentences)
The seven sentences in the morning practice are personal and situation-specific.
For other users they'd either be irrelevant or need to be their own.
Options:
- Let each user write and save their own set of sentences
- Make the label ("Seven Sentences") a user-chosen name
- Ship a neutral default set, editable from the morning tab

### 3. Privacy and account management
Storing someone else's mental-health journal is sensitive. Minimally needed:
- A short privacy policy (what data is stored, who can see it, how to delete)
- A way for users to **delete their account and all their data** from within the
  app (the database already cascades on user delete — just needs a UI button)
- A way to **export all their data** on request (the JSON exports do this, but
  it should be clearly surfaced)

### 4. Watch the free-tier limits
Supabase free tier is generous (500 MB DB, tens of thousands of monthly active
users) and won't pause with regular traffic. At real scale: move to a paid plan
and enable point-in-time backups.

### 5. Spam / abuse protection
Open public sign-up can attract bot accounts. Mitigation:
- Enable email verification on email-based providers
- Supabase has built-in rate limiting on auth endpoints
- For invite-only: restrict sign-up to an allowlist of email domains or
  specific user IDs in the RLS policies or a sign-up trigger

---

## Single-user hardening (opposite direction)

If the goal is to ensure **only you** can ever sign in:
- Add a `check (user_id = '<your-uuid>')` constraint or a policy guard to
  `app_data` — any other user's writes silently fail
- Or restrict the GitHub OAuth App to only your GitHub account (not a native
  Supabase option, but achievable via a sign-up trigger that deletes any account
  whose GitHub username doesn't match yours)

---

## Smaller quality-of-life ideas

- **Conflict resolution on sync.** Currently last-write-wins per dataset. For
  two active devices, a per-day merge (union of completed items) would be safer
  than a full-dataset overwrite.
- **Offline indicator.** Show when the app can't reach the cloud so the user
  knows their edits are queued locally.
- **Push notifications / reminders.** A morning and evening nudge to open the
  practice. Requires a service worker and the Notifications API.
- **Themes.** A dark-mode variant of the warm paper palette, for evening use.
