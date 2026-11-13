# USA Volleyball Partner API Documentation

Start here. This repository holds the documentation partners need to connect to the USA Volleyball Partner API.

## Which file do I need?

| I want to... | Use this |
|---|---|
| Learn how the API works: access, endpoints, filters, errors | [api-documentation.md](api-documentation.md) |
| Look up a field or filter: definition, data type, length, allowed values | [mms-partner-api-response-data-dictionary.md](mms-partner-api-response-data-dictionary.md) |
| Load the same field and filter list into a spreadsheet or tool | [mms-partner-api-response-data-dictionary.csv](mms-partner-api-response-data-dictionary.csv) |
| Receive notifications when records change | [webhooks.md](webhooks.md) |
| See an export of the test club's members | [sample-data/team-colorado-test-members.csv](sample-data/team-colorado-test-members.csv) |
| See what changed and when | [CHANGELOG.md](CHANGELOG.md) |
| Find an older document | [archive/](archive/) |

The two dictionary files work together and hold the same content. The `.md` file is easier to read, and the `.csv` file is easier to filter and sort.

## How to get access

1. Open the [Developer Portal signup page](https://usav-devportal.vercel.app/auth/sign-up). This is a temporary address. It will move to `developer.usavolleyball.org`, and the old link will redirect there. Your account, keys, and webhooks carry over. You will just need to sign in again.
2. Enter your email, confirm it from the email we send you, and set a password.
3. Sign in and fill out the **Request access** form.
4. **For testing, select region PT (Partner API Test Region).** It is near the bottom of the region list. Do not select other regions or clubs for testing.
5. USAV reviews each request. You will get an email when your request is approved or rejected. You can also sign in to the portal to check your status.
6. Once approved, create a Sandbox key under **API keys**. The full key is shown only once, so copy it right away.

You can also invite teammates and see your account activity in the portal.

## How to test

| Item | Value |
|---|---|
| Environment | Sandbox |
| Base URL | `https://api-sandbox.usavolleyball.org` |
| Interactive API reference | `https://api-sandbox.usavolleyball.org/reference` |
| Test region | `PT`, Partner API Test Region |
| Test club | `TCOPT`, Team Colorado Volleyball (Test) |

Send your key as a bearer token:

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" https://api-sandbox.usavolleyball.org/api/v1/clubs
```

With a PT key, this returns one club: Team Colorado Volleyball (Test).

### About the test data

- All of the people in the test data are synthetic.
- The sample CSV is an export of the test club's members.

## Production

Testing uses Sandbox. USAV will announce when Production data is available.

## How versions work

Each document has one file, and its name does not change when it is updated. The version on the `main` branch is always the current one.

- To see what changed, read [CHANGELOG.md](CHANGELOG.md).
- To see the exact changes to a file, open it and click **History**.
- To be notified of updates, click **Watch** at the top of this repository.

Documents in [archive/](archive/) are kept for reference only. Do not build against them.

## Questions

Ask in the partner Slack channel.
