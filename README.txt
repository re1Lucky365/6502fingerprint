i went down a very deep rabbit hole, and this is what followed me home.

working on a 6502 emulator, i wanted to add support for the undocumented opcodes.

one of these is ARR #imm.  (x6B)

i spent hours trying to figure out how this opcode works.

i even found a wonderful set of test vectors at
https://github.com/SingleStepTests/65x02

long story short, it is unstable, and i was unable to find any solid implementations that matched the test vectors.

also, the test vectors are incomplete.
they contain only 10,000 tests each, when the entire search space is 2^18.
(2^8 values for A * 2^8 values for #imm * 2 values for C * 2 values for D).

not enough for me to fully test any implementation i might come up with.

i also found perfect6502 at
https://github.com/mist64/perfect6502
which allowed me to generate a complete set of tests.

curiosly, it did not match well with ANY of the SingleStepTests.

as i was giving up (it's unstable, there is no single solution),
i realized that this could be leveraged to generate a unique fingerprint for different silicon versions.

i analyzed the test vectors for differences, and found 4 tests that together would allow me to differentiate between:
perfect6502
SingleStepTests/6502
SingleStepTests/nes6502
SingleStepTests/rockwell65c02
SingleStepTests/synertek65c02
SingleStepTests/wdc65c02

now, this doesn't mean i know what the fingerprints are;
i would need real silicon or a complete set of 2^18 for each to calculate that.

but i was able to calculate it for perfect6502, obvs.

maybe this will be useful to someone in some way.

maybe people will come in after me and add some fingerprints to the registry.

who knows?
Skip to content
web-platform-tests
wpt-pr-bot
Repository navigation
Code
Issues
31
 (31)
Pull requests
7
 (7)
Agents
Actions

 GitHub users are now required to enable two-factor authentication as an additional security measure. Your activity on GitHub includes you in this requirement. You will need to enable two-factor authentication on your account before November 21, 2026, or be restricted from account actions.

Pass auth to bugs.webkit.org properly- #274
#274
Merged
gsnedders
merged 1 commit into
web-platform-tests:main
from
gsnedders:fix-webkit-export-auth
15 hours ago
Conversation7 (7)
Commits1 (1)
Checks1 (1)
Files changed10 (10)
Conversation
@gsnedders
gsnedders
commented
yesterday
• 
Member
bugs.webkit.org only reads credentials from request parameters, not headers, so the X-BUGZILLA-TOKEN header was ignored and every request was made logged out. Logged-out responses now truncate email addresses (ews-feeder rather than ews-feeder@webkit.org), so the bot no longer recognised changes made by EWS or the commit queue and stopped approving WebKit export PRs.

Send the API key as the Bugzilla_api_key query parameter instead, and redact it when logging. Also ask for
exclude_fields=update_token, since authenticated responses otherwise include a per-user token.

create-secrets.js returned the secret as bugsWebkit while index.js reads bugsWebkitToken, so the token was never set in production.

Tests replay requests with a placeholder key. The real key is replaced with REDACTED both when saving fixtures and when matching them, so recording with a real key replays existing fixtures instead of duplicating them. The fixtures are re-recorded while authenticated.

The "wrong bugzilla reference" test passed without asserting anything: bug 202311 loads fine when authenticated, so the rejection handler never ran. Point it at a nonexistent bug so it covers the error path again.

See also, an alternative fix: #273

@gsnedders gsnedders mentioned this pull request yesterday
Fix WebKit export approval: verify landings against the WebKit repository #273
 Closed
@gsnedders
gsnedders force-pushed the fix-webkit-export-auth branch from ae1d46f to b4109fd 
yesterday
@gsnedders
gsnedders requested review from DanielRyanSmith and jgraham yesterday
gsnedders
gsnedders commented yesterday
create-secrets.js
        webhookSecret: await fetchSecret('github-webhook-secret'),
        githubToken: await fetchSecret('wpt-pr-bot-github-token'),
        bugsWebkit: await fetchSecret('wpt-pr-bot-bugswebkit-token'),
        bugsWebkitToken: await fetchSecret('wpt-pr-bot-bugswebkit-token'),
@gsnedders
gsnedders
yesterday
Member
Author
Given this had been broken, @DanielRyanSmith, can you check this actually exists and works?

@DanielRyanSmith
DanielRyanSmith
yesterday
Collaborator
I have confirmed the code here retrieves full emails back with this change

@adpsnake7-beep	Reply...
DanielRyanSmith
DanielRyanSmith approved these changes yesterday
@DanielRyanSmith
DanielRyanSmith
left a comment
Collaborator
Thanks for taking a look at this

lib/bugs-webkit.js
Outdated
@gsnedders
Pass auth to bugs.webkit.org properly 
8d5f383
@gsnedders
gsnedders force-pushed the fix-webkit-export-auth branch from b4109fd to 8d5f383 
15 hours ago
@gsnedders
gsnedders merged commit ac27ff7 into web-platform-tests:main 15 hours ago
1 check passed
@gsnedders
gsnedders deleted the fix-webkit-export-auth branch 15 hours ago
@DanielRyanSmith DanielRyanSmith mentioned this pull request 1 hour ago
Don't let one failed WebKit export check crash the poller #276
 Open
Merge info
Pull request successfully merged and closed
You're all set — the branch has been merged.



Add a comment
Comment
 

Add your comment here...
Remember, contributions to this repository should follow our GitHub Community Guidelines.
 ProTip! Add .patch or .diff to the end of URLs for Git’s plaintext views.
Reviewers
@DanielRyanSmith
DanielRyanSmith
@jgraham
jgraham
Assignees
No one assigned
Labels
None yet
Projects
None yet
Milestone
No milestone
Development
Successfully merging this pull request may close these issues.

None yet


Notifications
Customize
You’re not receiving notifications from this thread.
2 participants
@gsnedders
@DanielRyanSmith
Footer
© 2026 GitHub, Inc.
Footer navigation
Terms
Privacy
Security
Status
Community
Docs
Contact
Manage cookies
Do not share my personal information

