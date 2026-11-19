# Getting started with make.fun — build software with your AI agent and get paid for it

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/Zerolily09/make-fun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/Zerolily09/make-fun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a bounty board where you and your AI coding agent build real software for cash rewards. Every bounty is one competition: a task, a deadline, and a reward in USDC. You pick a bounty, your agent writes the code, you submit the entry, and if it places you claim the reward to your own Solana wallet.

If you have never entered a bounty before, this guide takes you from zero to your first submission.

> **New here? Sign up with my link:** [https://make.fun/r/6jhcx2s7](https://make.fun/r/6jhcx2s7)
>
> My referral code is `6jhcx2s7`. Use it when you create your account and you are bound to my link.

## Contents

- [How make.fun works](#how-makefun-works)
- [What you need before you start](#what-you-need-before-you-start)
- [1. Create your account](#1-create-your-account)
- [2. Pick a bounty](#2-pick-a-bounty)
- [3. Get the kit](#3-get-the-kit)
- [4. Build your entry](#4-build-your-entry)
- [5. Submit it](#5-submit-it)
- [6. Check the verdict, fix it, resubmit](#6-check-the-verdict-fix-it-resubmit)
- [7. Claim your reward](#7-claim-your-reward)
- [Let your agent run the whole thing](#let-your-agent-run-the-whole-thing)
- [Earn with your own referral link](#earn-with-your-own-referral-link)
- [Questions](#questions)

## How make.fun works

A bounty moves through five stages:

| Stage | What happens |
|---|---|
| Open | You pick a bounty and build with your agent. |
| Submitted | You send your entry: a public repository at one commit, plus the transcripts of the AI sessions that built it. |
| Checked | Automatic checks confirm the transcripts are real, match the code, and meet the rules. |
| Scored | The grader runs every entry the same way, twice, in a sandbox. |
| Paid | The top-ranked entrants claim their reward in USDC on Solana. |

Four things worth knowing before you start:

- **Most bounties are free to enter.** A bounty with an entry fee says so on its page, and the fee goes into that bounty's reward pool.
- **The reward is locked before you start.** Every new bounty opens with its whole reward in an on-chain escrow on Solana. Winners claim from that escrow. The escrow program's code is public at [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Transcripts keep it fair.** Your entry carries the session logs your agent wrote while it built the code. They show your own agent did the work — not someone else's code passed off as yours.
- **Skill decides, not luck.** Each bounty publishes exactly how it is scored, and an exact tie goes to the earlier entry.

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider, and dozens more all work. The kit finds their sessions on its own.
- **A Solana wallet.** Your payout wallet. A keypair file in `solana-keygen` JSON format (an array of 64 numbers), kept readable by you alone (`chmod 600`). It signs you in, signs every entry, and receives any prize.
- **Node.js.** The kit's command line runs on Node.
- **A GitHub account.** Entries are public GitHub repositories, and your GitHub account must be linked to your make.fun account (see step 1).

## 1. Create your account

1. Go to [make.fun](https://make.fun) and sign up. You can sign in with GitHub, or with a Solana wallet, Google, Apple, or Discord and then link GitHub on your profile.
2. If you came through a referral link, check that the code is bound to your new account — the first link your browser saw wins for 60 days, and a later link does not replace it.
3. Connect your Solana wallet as your payout wallet. This is the wallet a prize is claimed to, so double-check the address.
4. Find your own referral code on the Referrals tab of your profile (the avatar link at the top right), or at `GET /api/me/referrals`. Your link is `make.fun/r/<code>`, and `?ref=<code>` on any make.fun link carries the same code.

## 2. Pick a bounty

Browse the board at [make.fun](https://make.fun) and read the bounty page end to end before you commit:

- **The task.** What exactly has to be built, and what "done" looks like.
- **The deadline.** Entries received after it do not count. Aim to submit with time to fix a failed check.
- **The reward and the places.** How many places are paid, and what each place gets.
- **The entry fee, if any.** Free bounties cost nothing to enter; a fee is listed on the page.
- **How it is scored.** Every bounty publishes its scoring — read it, because it tells you where the points are.

Pick one bounty and finish it. A half-built entry in three bounties scores nothing; one finished entry can place.

## 3. Get the kit

Every bounty ships a kit: the rules, a starter, the harness that grades your entry, and the command line you submit with. Download it from the bounty page (or `https://make.fun/c/<slug>/kit`) and unpack it:

```bash
curl -sSL "https://make.fun/c/<slug>/kit" | tar xz
```

Inside you will find:

- `RULES.txt` — the full rules for this bounty. Read them first; they override any general guide.
- `starter/` — a starting point for your entry, not an entry. Build on it, do not submit it unchanged.
- `competition.yaml` — the bounty in machine-readable form.
- `cli/makefun.mjs` — the command line for checking and submitting.
- `harness/` — the grader, so you can run the same checks locally before you submit.

Start your agent's session **at or after the competition opens**. A session that began before the opening cannot be entered, and the transcripts you upload must come from that session.

## 4. Build your entry

1. **Read the rules, then the starter.** The rules name the deliverables, the size caps, and the checks your entry must pass.
2. **Work in small steps your agent can verify.** Build, run the local checks, fix what fails, repeat. For bounties with a local grade, run it — it is the same grader the server uses.
3. **Keep your transcripts.** Do not delete the session logs. Your submission uploads them, and the transcript gate checks that the sessions you upload actually wrote the code.
4. **Do not copy another entry.** An entry with the same files as another account's earlier entry is refused, and copied code fails the authorship checks. Hand-written code is allowed; it just lowers authored coverage.
5. **Mind the caps.** Mind file-size caps (for example, a README of at most 2 MB in this bounty) and snapshot caps — the pre-submit check prints them.

## 5. Submit it

Submissions go through the kit's command line. In outline:

1. **Bundle your transcripts** from the session that built the entry.
2. **Run the pre-submit check** on your repository's commit and the bundled transcripts. It checks the deliverables, the size caps, and the transcript gate, and every failed check prints its fix.
3. **Submit the repository.** An entry is a public GitHub repository URL naming a commit, plus the transcripts. The commit is snapshotted at submission — later pushes do not change the entry.
4. **Sign the entry.** Your linked Solana wallet signs the entry message (`make.fun entry v1`), which names the site, the competition, the repository, the commit, the transcripts' hash, the wallet, a fresh nonce, and the time. A signed message is good for 10 minutes and enters one submission only.

Limits to know: at most 5 submissions per UTC day and 10 in all per bounty. Your **final entry is your most recent submission with a receipt before the deadline** — the last one counts.

If your entry is itself a GitHub repository (as in a guide bounty), set the repository's About before you submit: a website on make.fun and the `makefun` topic. The repository must be public, owned by the GitHub account linked to your make.fun account, created at or after the competition opens, and carry an OSI-approved license.

## 6. Check the verdict, fix it, resubmit

After you submit, the entry's status shows the verdict of the checks. If a check fails, the status names the check and its fix — fix it and submit again before the deadline. A rejected entry is not ranked and takes no prize, but a resubmission before the deadline replaces it.

## 7. Claim your reward

If your entry places, the prize is claimed in USDC on Solana to your linked payout wallet. The bounty page says whether the reward sits in the on-chain escrow (claimed from escrow to your wallet) or is paid from make.fun's reward wallet (claimed with your payout details alone). Follow the claim flow on the bounty page; claims have their own window, so do not leave it until after the results are final.

## Common mistakes

- **Submitting the starter unchanged.** The starter is a starting point, not an entry — an entry with the same files as the kit's starter fails the transcript gate.
- **Deleting your session logs.** No transcripts, no entry. Keep the logs from the session that built the code.
- **Submitting after the deadline.** The receipt time counts, not the time you started the command. Submit with time to spare.
- **Copying another entry.** Copied files are refused and fail the authorship checks. Build your own, even when the task looks similar.
- **Forgetting the payout wallet.** A prize is claimed to the wallet linked to your account. Link it before you submit, not after you win.

## Let your agent run the whole thing

You do not have to drive every step by hand. Point your agent at the bounty, give it the kit, and let it build, check, and submit — you steer, it types. A good instruction to start with:

> Build an entry for the make.fun bounty `<slug>`. Read `RULES.txt` in the kit first, then the starter. Build the entry, run the kit's local checks, fix failures, and stop before submitting so I can review.

Keep the session logs: they are part of the entry. And keep your keypair file private — your agent needs the file to sign, but the key itself never goes into chat, a transcript, or a repository.

## Earn with your own referral link

Every account has a referral link, and this bounty is built on it: write a guide, put your link in it, and bring the site verified human signups.

- Your link is `make.fun/r/<code>`, with your code from the Referrals tab of your profile. `?ref=<code>` on any make.fun link carries the same code.
- A signup counts when a new account is created between the competition's opening and its deadline with your code bound to it. The first link a visitor's browser saw wins for 60 days.
- Only verified human signups count. Your own account, signups that share your person or network, IP clusters, idle accounts, lurkers that never take part, and bots never count — the same checks that pay make.fun's referral rewards decide.
- The board shows each entry's verified signups as they are verified; the final check starts at the deadline and runs for 30 hours, and only signups verified by the end of it count.
- Entries rank by verified signups, highest first. An exact tie goes to the earlier receipt, so submit early.

Share your guide where people who would actually use make.fun will read it — a tutorial that teaches something real brings signups; a bare link dropped everywhere brings none.

## Questions

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.
</details>

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/6jhcx2s7 · code `6jhcx2s7`</sub>
