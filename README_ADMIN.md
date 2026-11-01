# Instructor setup for the repo-request bot

The workflow `.github/workflows/provision.yml` runs when a student opens a "Repo request"
issue. It creates `se3082-a3-<StudentID>` (private) from `se3082-july/se3082-a3-template`,
invites the student with **write** access, gives the `instructors` team **read** access,
replies, closes the request and deletes it.

## 1. Create the token (organisation owner, once per semester)

GitHub → your avatar → **Settings → Developer settings → Personal access tokens →
Fine-grained tokens → Generate new token**

| Setting | Value |
|---|---|
| Token name | `se3082-provision` |
| Resource owner | **se3082-july** |
| Expiration | end of semester |
| Repository access | **All repositories** |
| Repository permissions | **Administration:** Read and write · **Contents:** Read-only · **Issues:** Read and write · Metadata: Read-only (automatic) |
| Organization permissions | **Members:** Read-only |

If the organisation requires approval for fine-grained tokens, approve it under
**Organisation settings → Personal access tokens → Pending requests**.

## 2. Store it as a secret

`start-here` → **Settings → Secrets and variables → Actions → New repository secret**

- Name: `PROVISION_TOKEN`, value: the token.
- Optional: `ALLOWED_STUDENT_IDS` – the class list, one student ID per line. Requests with
  IDs not on the list are rejected. Kept as a secret so IDs are not published.

Optional variable (same page, **Variables** tab): `KEEP_ISSUES` = `true` keeps requests
instead of deleting them.

## 3. Handling problems

A failed request stays open with the label `needs-instructor` and a comment explaining why.
After fixing the cause, add the label `retry` to run it again.

Each repository's description records the GitHub account it was issued to, e.g.
`SE3082 Assignment 03 - IT24999938 [@username]`.
