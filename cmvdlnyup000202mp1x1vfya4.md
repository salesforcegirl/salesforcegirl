---
title: "Heroics are not a deploy plan"
seoDescription: "A deploy that only works because people stay up all night isn't a process. How we went from heroics to a runlist, and 66% fewer bugs in hypercare."
datePublished: 2026-10-18T09:05:13.014Z
cuid: cmvdlnyup000202mp1x1vfya4
slug: heroics-are-not-a-deploy-plan
cover: https://cdn.hashnode.com/uploads/gql/6aa40b4d011a85bf7a48aa10/d6241ec9-945d-469c-be0d-fd9ec075d20b.png
ogImage: https://cdn.hashnode.com/uploads/gql/6aa40b4d011a85bf7a48aa10/d6241ec9-945d-469c-be0d-fd9ec075d20b.png
tags: tdd, testing, devops, agile, software-engineering, salesforce, leadership, cicd

---

The first coordinated deploy I watched closely wasn't a big one. A site was moving from being hosted in our CRM to being hosted by IT, and a few teams had to line up to make it happen. It was my first real look at how IT and the teams around it prepared for a deploy together.

So I took notes. As I always do.

IT had copied the production engineering rules from a much bigger organization, and they were too big for its britches. The process was heavy enough that the developer knew exactly how to cut around it, and got approvals on the fly for things that should have had real checks. Reviews were rubber stamps. Approvals in git were a click.

Rubber-stamp reviews don’t prevent the damage. They just move it to the deployment window.

And when it went sideways, everyone went straight into fight-or-flight. Reactive mode has never fixed anything, and it didn't then. Eventually I stepped in and called for order: *let's pause and look at the forest, not just the tree in front of us.* Yes, it's on fire. But why? Could it be worse than we think, because we can only see what's right in front of us?

You can only take the pulse of the whole scope when you're calm. I wrote that down too. I take lessons from my own experience and from watching the people around me, which is why I'm a strong advocate for regular retros.

## Where my rules came from

I didn’t start out believing in process. I learned it the way most people learn to swim: by being thrown in.

In my early days, deploys meant change sets and Eclipse. You built it, you pushed it, you hoped.

Then I joined a company that moved at a speed I’d never seen. True CI/CD, releases going out every hour, git, Jenkins and Ant doing the heavy lifting, and two code reviews before anything could reach the next branch. Demos were the sign-off. Everything was turned up a notch, and I felt like I was swimming in open sea knowing only the doggy paddle. I had to learn fast or drown.

After that came a mature company with the opposite problem. A CAB meeting. Locked runlists. Admin-only access to the upper environments. So much red tape that I don’t think even they knew why some of it was there, and nobody ever went back to ask. It was the classic “better safe than sorry”: one person broke something once, so now everyone does this forever. They still had rubber stamps. The people approving changes in the CAB relied on the teams’ explanations and rarely dug deeper. With that much standing between a change and production, they could release twice a week, and only with more sign-offs than a purebred at a dog show.

I took notes there too. And I asked why. I pushed back, and worked out which checks were useful and which were red tape. That company is also where I learned about audits and root cause analyses, and why some of those checks protected the team when it came under fire.

On the surface, the extra tasks and controls look like micromanagement or busy work. But when one of them saves you in an RCA, or makes a rollback a plan instead of a panic, or keeps your evenings your own, it’s the hero. It gives you transparency and accountability. The people who rail against it most are usually the ones trying to fly under the radar, or stuck in fight-or-flight: they don’t want the scrutiny, and they don’t have time to change how they work.

It’s the manual upload at midnight that someone does every night so sales has fresh data in the morning. They’ll keep doing it forever. Or you take one sprint to automate it, and you get that person’s evenings back, plus logs, reports, error alerts, and a process anyone can see. Forest, not just the tree in front of you.

## Phase one: heroics

Then came a big project, in two phases, and phase one shipped on heroics.

The code got built. The tests didn't. Salesforce won't let you deploy without enough code coverage, so for a whole sprint I sat with my lead developer, past midnight, writing test classes for code that had already moved on. We covered the minimum lines needed for the deploy to go through. We didn't test whether anything actually worked. Flimsy at best, but at least they had asserts.

I've seen worse. Tests that create data and check nothing. A "test" that calls one method containing a thousand lines of `i++`, purely to inflate the coverage number. I've seen it in every org I've touched.

We got phase one out, buggy but out. A deploy that only works because people stay up all night isn't a process. It's a debt, and someone pays it.

## Phase two: the numbers fight

By the time phase two came around, I could see how bad it was going to be.

Estimates from one of the other teams weren’t grounded in anything tangable. Nobody had put real thought into them, and every status update said "90% done" with no progress anyone could see. Asking for demos and help with slides and content for read outs fell on deaf ears. Inside my own team, I was managing pressure that pushed the developers past their limits, and a people-pleasing habit that had them undercutting their own estimates to hit the deadline. They had taken on double their normal sprint load and were still working intake on top of it.

Undercutting an estimate doesn't make the work smaller. It just sets everyone up to fail later, with less time to recover.

So I fought the deadline with numbers and data instead of hope. The biggest one: leaving test classes to the end was being treated as a lever to "save time for UAT". It isn't. Writing tests after the developers have long moved on from the code takes about twice as long, and the tests come out worse, because the developer no longer remembers the work.

Good test-driven development happens while the developer is in the code. They know the caveats, the false positives and the nulls. Writing tests as they work is also how they find the use cases that power the smoke tests: the checks in the UI that show whether it truly works.

Saved to last, the developer has to dig back through tickets long since closed to remember what the code was even for. That's how you end up back at midnight writing tests for coverage, not function. I'd done that sprint already. I wasn't doing it again.

And I worked on the part that isn't a spreadsheet: helping the developers speak up, give honest estimates, and protect their own time, even when the pressure said otherwise. A burnt out developer isn't going to do anyone any good, least of all themselves.

## The rules I kept

Everything from those notes, phase one and phase two, became the way I run deploys now, along with rules I've seen work at past companies and lessons from the projects that didn't:

*   **One runlist, in execution order, across every team.** Including the waits and handoffs, like an ID created in one system that another team needs before it can start.
    
*   **Know which sections mean the deploy is hosed.** If one of those fails, you don't push through. You stop.
    
*   **The rehearsal puts real numbers on estimates.** A test run or UAT run is where an estimate becomes something practiced, not something guessed. Some systems barely have a test environment, so you plan for that too.
    
*   **Rollback, not heroics.** A rollback plan that's actually tested, or at least walked through by the whole project team, beats a hero at 2am every time.
    
*   **A freeze window the business agreed to.** Teams don't change environments during it, because backups and data pulls lose anything entered after them. And changes in the org mean merge conflicts, and failed deploys of things that passed in the test run.
    
*   **Sign-off agreed up front.** UAT goes stale fast without it.
    
*   **Deploy defensively.** Systems are messy and tests are sometimes flimsy. Accept that, and plan around it instead of pretending it isn't true.
    
*   **Trust your experience and gut.** If something doesn't sit right, lean into that.
    
*   **Communicate the progress.** A deployment Slack channel and a video bridge open for the whole deploy. Report what's actually happening, share the tracker and the bridge with stakeholders, and give them the sign-offs they own and a seat at the go/no-go table.
    

## The result

Phase two went live on time, on a Monday morning, with only minimal issues.

Compared with phase one, it had **66% fewer bugs by week two of hypercare** (10 against 29) and **67% fewer in the first 90 days** (33 against 99). Measured against near-identical 12-month baselines (248 vs 245 bugs a year): phase one ran at 1.6× its normal bug rate after go-live, phase two at 0.55×.

Same company, same platform, many of the same people. The difference was how we prepared, and that we stopped relying on heroes.

## Progress is finished steps

The tool behind all of this was a spreadsheet: a deploy runlist and go/no-go tracker. Two ideas in it did the most work.

**Every estimate says where it came from.** *Practiced*: someone has actually run this step. *Owner's guess*: someone thinks it'll take this long. *Missing*: nobody knows. You can't hide a guess behind a confident number when the sheet says it's a guess.

**Progress is finished steps, never a percentage someone reports.** "90% done" doesn't mean anything. Twelve of fifteen steps finished does.

And the gates hold every team to the same rule, so nobody gets a pass because they're louder or busier or short staffed.

## What I'm building next

That spreadsheet is turning into **DeployLogs**: a Google Sheets app for deploy runlists and go/no-go calls, the way Saasy Logs grew out of a debug log I was tired of reading by hand. It's early days.

Heroes are great. You just shouldn't need one to ship.