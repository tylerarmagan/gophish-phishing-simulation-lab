# Step-by-Step Guide: Kali Linux and Gmail Self-Test

This version is designed for beginners learning Linux commands and GoPhish workflow. It sends one or two clearly labeled lab messages to an account owned by the student. Gmail may filter or restrict simulated phishing content; Mailpit is a better option for a larger classroom exercise.

## 1. Safety Boundary

- Use a dedicated test Gmail account, not school, work, or primary personal email.
- Send only to an account you own.
- Use fictional branding and a dummy training code.
- Never collect a real password, MFA code, recovery code, or personal information.
- Do not send to classmates unless an instructor has created an explicitly authorized exercise.

## 2. Confirm Kali Internet Access

Open a terminal and run:

```bash
ip addr
getent hosts smtp.gmail.com
```

The first command displays network interfaces. The second verifies DNS resolution for Gmail's SMTP service.

## 3. Install GoPhish from Kali

Update the package index:

```bash
sudo apt update
```

Install GoPhish:

```bash
sudo apt install -y gophish
```

Confirm the package and executable:

```bash
command -v gophish
dpkg -s gophish | grep -E "Status|Version"
```

These commands teach students how to locate an executable and inspect an installed Debian package.

If the Kali mirror returns `503 Service Unavailable`, the mirror is temporarily unavailable. Retry later:

```bash
sudo apt clean
sudo apt update --fix-missing
sudo apt install -y gophish
```

Do not mix the Kali package with a separately downloaded ZIP installation.

## 4. Launch GoPhish

The simplest method is:

1. Open Kali's **Applications** menu.
2. Search for **GoPhish**.
3. Launch it.
4. Enter the Kali administrator password if prompted.
5. Keep the terminal window open.

Alternatively, launch it from the terminal. On systems using the observed Kali package configuration:

```bash
sudo gophish --config /etc/gophish.config.json
```

If that path does not exist, discover the packaged configuration:

```bash
dpkg -L gophish | grep -E 'config.*json'
```

Students normally do not need to edit the configuration for a same-VM self-test. The startup terminal shows the actual admin and phishing-server addresses.

## 5. Sign In to the Admin Console

Open the address shown in the terminal, commonly:

```text
https://127.0.0.1:3333
```

Because the local admin certificate is self-signed, Firefox may show a warning. Continue only after verifying that the address is your own local GoPhish service.

Sign in with:

```text
Username: admin
Password: temporary password printed in the GoPhish terminal
```

Change the temporary password and do not reuse a school, work, or Gmail password.

## 6. Create a Google App Password

Gmail does not support using the normal account password in this setup.

1. Sign in to the dedicated sending Gmail account.
2. Enable **2-Step Verification**.
3. Open Google App Passwords.
4. Create an App Password named `GoPhish Lab`.
5. Copy the generated 16-character value into a password manager.

Google may display spaces for readability. Enter the value in GoPhish without spaces. If App Passwords are unavailable for the account, do not bypass the restriction; use Mailpit instead.

## 7. Create the Gmail Sending Profile

Open **Sending Profiles → New Profile** and use:

| Field | Value |
|---|---|
| Name | `Gmail Self Test` |
| Interface type | `SMTP` |
| SMTP From | Exact dedicated Gmail address |
| Host | `smtp.gmail.com:587` |
| Username | Exact dedicated Gmail address |
| Password | 16-character App Password without spaces |
| Ignore certificate errors | Disabled |

Begin with only the raw address in **SMTP From**:

```text
student.lab.account@gmail.com
```

This avoids malformed display-name syntax during troubleshooting.

Use **Send Test Email** and send only to an account you own. Check Inbox, Spam, and All Mail.

### Common SMTP Errors

`535 5.7.8 Username and Password not accepted`

- Confirm that 2-Step Verification is enabled.
- Generate a fresh App Password.
- Confirm that the App Password belongs to the username entered.
- Remove spaces from the App Password.
- Use the complete Gmail address as the username.

`555 5.5.2 Syntax error`

- Retype the SMTP From address manually.
- Use only the Gmail address without a display name.
- Remove custom headers.
- Confirm `smtp.gmail.com:587`.
- Ensure the test-recipient field contains one plain email address.

Do not repeatedly retry incorrect settings; fix the field first.

## 8. Create the Self-Test Group

Open **Users & Groups → New Group**.

```text
Group name: Self-Test Group
First name: Student
Last name: Lab
Email: an address you own
Position: Student
```

Confirm that the group contains exactly one recipient.

## 9. Create the Fictional Landing Page

Open **Landing Pages → New Page**.

1. Name it `Northstar Training Portal`.
2. Paste the HTML from [`templates/fictional-training-page.html`](../templates/fictional-training-page.html).
3. Enable **Capture Submitted Data** only for the dummy training code.
4. Keep **Capture Passwords** disabled.
5. Use a harmless redirect such as `https://example.com`.
6. Save the page.

Do not use **Import Site** to copy a real login page.

## 10. Create the Email Template

Open **Email Templates → New Template**.

1. Name it `Northstar Self Test`.
2. Use a clearly labeled subject such as `[LAB] Northstar Training Notice`.
3. Paste the HTML from [`templates/fictional-training-email.html`](../templates/fictional-training-email.html).
4. Confirm that the template includes `{{.URL}}`.
5. Save the template.

The `{{.URL}}` placeholder lets GoPhish create the unique campaign link.

## 11. Determine the Local Campaign URL

Read the GoPhish startup terminal. If the phishing server is listening on port 80 and Gmail will be opened inside the same Kali VM, use:

```text
http://127.0.0.1
```

If it is listening on port 8080, use:

```text
http://127.0.0.1:8080
```

Because `127.0.0.1` always means the device currently opening the link, the Gmail message must be opened inside the same Kali VM.

## 12. Launch the Campaign

Open **Campaigns → New Campaign**:

| Field | Selection |
|---|---|
| Name | `Authorized Gmail Self Test` |
| Email Template | `Northstar Self Test` |
| Landing Page | `Northstar Training Portal` |
| URL | Local URL shown by the GoPhish configuration |
| Sending Profile | `Gmail Self Test` |
| Groups | `Self-Test Group` |

Before selecting **Launch Campaign**, verify:

- One owned recipient only
- Dedicated test sender
- Local campaign URL
- Fictional template
- Password capture disabled
- Dummy data only

## 13. Complete the Self-Test

Inside the same Kali VM:

1. Open the receiving Gmail account in Firefox.
2. Open the lab message.
3. Inspect the sender, subject, link destination, requested action, and Gmail warnings.
4. Click the controlled local link.
5. Enter a dummy code such as `LAB-482`.
6. Submit the form.

## 14. Review the Results

Return to the campaign dashboard and review:

- Email Sent
- Email Opened
- Link Clicked
- Submitted Data

Open tracking may be missing or misleading because Gmail can proxy or block tracking images. Click and dummy-submission events are more useful for this self-test.

## 15. Clean Up

1. Save only redacted screenshots.
2. Delete the campaign and target group.
3. Delete the Gmail sending profile.
4. Delete the test email.
5. Stop GoPhish with `Ctrl+C`.
6. Revoke the Google App Password.
7. Remove any local database or logs that are no longer required.

## Classroom Recommendation

For a multi-student club exercise, use Mailpit instead of Gmail. Mailpit avoids individual Google account setup, App Passwords, spam filtering, sender-reputation effects, and accidental delivery outside the lab.

