---
layout: post
title: droppin crits
categories: software
---

moar fun times at the work place doing appsec and pentesting after being challenged to hack an internal web app.

### // stateless spa retreat

so, the cto and product team of the org decided they were going to parade around telling everyone about how `SaaS` is dead and whatnot, and how vibe coding is the future. they had been working on some internal apps and public facing apps for some time now, but never really included anyone else in the org, including the security team. by the time i was included in the mix, they had already launched to prod while claiming it was still in dev.

on an internal meeting where they showcased the app, i was able to grab some of the details of the web application and began reviewing the source code. initially, i couldn't fathom why anyone would put sensitive details in the main index.js file's source code, such as backend authentication infrastructure and API endpoint details, and i chalked it up to an error that their ai generated and they just ran with it. i tried pointing this out to the product team, however, i was corrected by cto-man that this is the design they went with--a stateless spa that stores jwt data in `localStorage`--so everything is as designed. dafuq?

anyway, i tried pointing out the inherent flaws in the architecture, but i was met with frustration- and sarcasm-laden retorts on cto-man's part, as if to suggest that i had no understanding of what it was they were doing, and he doubled down on everything. he even went as far as to challenge me to just hack into the app--i triple-dog-dare you, but without saying it in so many words.

challenge accepted.

### // critting to win

we started off with an internal audit of all 3rd party platforms to make sure everything was up to snuff. there were some risky misconfigurations found, a high number of crits, but nothing compared to the swiss cheese that is the internal apps they are developing.

second week was the start of the pentest, and boy did i start out strong. i still can't belive they decided to go with a stateless spa design for their frontend that is meant to fetch and present EHR and PII.

- **credential reuse across platforms** (high, cwe-522) — i bought a gift card for $10 dollars and this gave me access across multiple services. this by itself isn't bad up until i pivoted over to the internal services that i wasn't supposed to have access to.
- **infinite refresh token reuse** (critical, cwe-613) — refresh tokens never expire and don't rotate. steal one from localStorage or a log, and you have indefinite access. no forced logout, no revocation path.
- **authentication bypass** (critical, cwe-287) — i overwrote the execution flow to route me to the token refresh endpoint, bypassing the need to actually authenticate and that let me skip auth entirely.
- **privilege escalation** (critical, cwe-269) — mass assignment vuln where i just manipulated the objects to say i was a super admin instead of just an employee.
- **sensitive pii disclosure** (critical, cwe-200) — unprotected api endpoints dumped full provider PII with zero auth. nothing else to say here. dev team was quick to address this issue, though.
- **patient id enumeration** (high, cwe-284) — unauthenticated idor on patient records. not damaging by itself, but can be useful for later testing.
- **provider pii disclosure** (high, cwe-200) — second look at the same issue above after dev team issued the fix. still no auth and leaking some provider PII (names, email addresses, and phone numbers) without auth or rate limiting.
- **websocket message leakage** (critical, cwe-284) — websocket channels just broadcast messages to all users without having to do anything other than signing into the website.
- **token storage weakness** (high, cwe-922) — tokens stored in localStorage, fully readable/modifiable by any client-side script. xss jackpot, if you can find one.
- **firebase config exposure** (medium, cwe-200) — firebase project id, api keys, auth domains all hardcoded client-side. instant backend access for anyone who cares to look. i could have spent more time on this, since push messages could be sent from the client, but i didn't have much of a window to research the potential.
- **credential disclosure** (critical, cwe-209) — error messages and debug endpoints printed db creds, api keys, and internal secrets in plaintext. this was a fun find. too bad it was on a dev instance (a publicly accessible one) and not prod.
- **ehr api access** (high, cwe-522) — once creds were compromised (easy via above), full sandbox ehr data was exposed—but it was mostly dummy records with some legit employee data. at one point, a month ago or so, they were routing live clients to the dev instance by accident. too bad they had already fixed it by the time i found this.

### // eof

this whole pentest reminded me of the early 2000s when i'd screw around with random, ameteur php websites for lulz. shit was easy back then. i'm glad they kept that spirit alive, lel.

cto-man decided to call me "hacker of the month" in slack after dropping all of this mess on them.

moral of the story? don't kick a gift horse in the dick...

stay schwifty, bitches.

-an0malous
