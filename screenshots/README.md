# Screenshot Checklist

Screenshots make the project easier for recruiters and students to understand, but every image must be reviewed before publication.

## Recommended Screenshots

1. DigitalOcean droplet overview with IP addresses and account identifiers redacted
2. Firewall rules showing restricted administrative access
3. GoPhish dashboard with recipient addresses and campaign IDs redacted
4. Redacted campaign timeline showing sent, clicked, and dummy-submission events
5. DNS validation output for SPF, DKIM, and DMARC with the real domain replaced if necessary
6. Redacted message headers showing authentication results
7. Browser certificate details for the controlled TLS endpoint
8. Kali Applications search result for GoPhish
9. GoPhish startup terminal with passwords and identifiers covered
10. Fictional Northstar email and landing page

## Redaction Checklist

Before committing an image, remove:

- SMTP usernames, keys, and App Passwords
- GoPhish administrator password
- Real email addresses
- Real recipient names
- VPS public IP address, if the server still exists
- Account IDs and billing information
- Tracking IDs and unique campaign links
- Session cookies and browser profile information
- Submitted data, even if it was intended to be dummy data
- Any domain that remains connected to active infrastructure

Use descriptive filenames such as:

```text
01-architecture-overview.png
02-email-authentication-results.png
03-redacted-campaign-timeline.png
04-fictional-training-page.png
```

Do not commit screenshots merely to make the repository look fuller. Each image should prove a specific technical outcome described in the README.

