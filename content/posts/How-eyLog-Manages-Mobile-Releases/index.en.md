---
title: "How eyLog Manages Mobile Releases"
date: 2025-01-20T08:01:10+06:00
description: eyLog Release cycle
hero: images/site/eyw.png
menu:
  sidebar:
    name: How eyLog Manages Mobile Releases
    identifier: eyr
    weight: 3
tags: ["eyLog", "Release"]
categories: ["basic"]
---

### **Context**

Consistently launching updates on the App Store and Play Store can be more complicated than it seems, particularly managing multiple apps at the same time. Exploring the workflows of different teams is fascinating. By identifying commonalities and distinctions, we can uncover potentially beneficial strategies. Here is an overview of eyLog's mobile release management.

---

### **Overview of Release cycle**

Our release cycles are pretty similar to what many other companies do. Here’s a quick look at how we handle things:

- We release a new version of the app every month, so we have a release cut every month
- Testing starts as soon as we cut a release. We have about a week to fix any major issues, and we do this by adding specific fixes to the release branch
- After testing and fixing are done, we submit the app for App Store review, usually in the middle of the month. This timing gives us some extra time in case there are any rejections or delays in the review process.
- Once we get approval, the build is kept ready for release until the end of the month.
- On release day, we start by rolling out the update by phased release for Automatic Updates to small perecentage of our users. This helps us catch any big problems that we might have missed during testing.
- After carefully watching how the release is performing by usinng new reliac , we speed up the rollout to all users by Day 7
- While this is happening, we’re already working on the next release, so it’s a continuous cycle.

---
### **Managing Communication**

To keep communication clear and organized during releases and rollouts, we create a Cliq channel for each release, like #ios-release-6-1-0. This helps centralize updates and discussions about a specific release, which reduces distractions in other channels and makes it easy to find information about past releases if needed.

Having a dedicated Cliq channel is especially useful when we need to issue hotfixes. Since we release updates weekly, a hotfix for an existing issue means that two releases are happening at the same time. Keeping each release’s communication in its own channel helps avoid confusion. For example, anything related to a hotfix for version 6.1.0 would be discussed in #ios-release-6-1-0, while discussions about the upcoming version 6.2.0 would take place in #ios-release-6-2-0.

---

### Testing Release Candidates  

Our apps are so big that it's not possible for just one person or even a small group to handle all the testing for release candidates. The team that focuses on quality assurance (QA) can't keep up with the heavy weekly testing needed either. With so many changes and new features being developed quickly, it’s hard to make sure the right things are being tested correctly. The people who are actually building the features know best what’s new, what’s changed, and how to test it.  

That’s why we rely on a group of engineers called "watchdog" consist of backend , frontend & mobile engineers for testing release candidates. Each watchdog is in charge of testing their own part of the app, whether it's a feature or section, and they fix any issues found during testing or delegate fixes to others. Each part generally matches up with our product teams—for example, the login team tests the login feature of the app. Each watchdog  has a specific set of tests they must run before they can approve their component. Every component has to be approved before we can submit the release for review.

---
### Handling Issues During Release Candidate Testing  

If a watchdog finds a problem during release candidate testing, they work with their team to figure out the issue and then create a fix on our main branch using a standard pull request. Once the fix is merged, it might be eligible to be added to the release branch through a process called cherry-picking. However, because adding changes at the last minute can be risky, we have strict rules about what can be included in a release.   

We allow fixes for serious problems or new bugs that affect users' experience, but we don’t allow minor bug fixes that don't impact users and we definitely won't use this process to add new features that missed the deadline. We developed a system for reviewing fix requests, where teams submit requests that explain the issue and provide evidence, which are then checked by the lead.  

After the release, the process for making a hotfix is similar to requesting a cherry-pick during a release cycle, but the rules are even stricter. Creating a hotfix requires more effort and could interfere with upcoming normal releases. If we discover a bug late in the release cycle—like after the app has been submitted for review but before it goes live—the decision to fix it will depend on the same strict rules as for post-release hotfixes.   

Even if the update isn't public yet, it could be waiting for review or already approved. To fix it, we’d need to reject the current build and resubmit the app. Since this could delay the release, we evaluate whether a fix is necessary and how to handle it on a case-by-case basis. We might choose to reject the build, apply a fix, and then resubmit, or allow the scheduled release to happen and issue a hotfix afterward. Alternatively, we might decide that the bug isn’t serious enough to need a hotfix, waiting to fix it in the next release cycle.

### **Monitoring rollouts post-release**

Lead and respective developer both are responisble for monitoring post release. We use New Reliac and Firebase to trackdown crashes and health. If lead sees something unusual, he delegates respective developer to take a deeper look and make fixes as needed. But if no problems arise, we can automatically go to a full rollout.

