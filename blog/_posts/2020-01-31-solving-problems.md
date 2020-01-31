---
layout: goal
title: On solving engineering problems as a member of a team
date_updated: 2020-01-31
---

# On solving engineering problems as a member of a team
Resolving engineering problems can be incredibly difficult, one thing that can help is having a structured approach to how you solve those problems. This post is going to present a path to solving problems as a member of a software engineering organization.

To be clear, the type of problem solving I'm referring to is the "Oh no, I have absolutely no idea" feeling when you first open a feedback ticket against a code base with which you only have a passing familiarity. It’s the type of problem where there isn’t a straightforward solution and so requires some investigation and buy in from your team. Going from that feeling to a solution which you and your organization are happy with is a journey. I typically structure that journey as follows:

1. Define and share the problem statement.
2. Propose solutions.
3. Collaborate with your team to solve the problem.
4. Document the final state.

Before you start, let's get rid of that sinking feeling. **Take a deep breath.**

Nobody is expecting you to propose the "correct" or optimal answer immediately. Your responsibility as a member of the team is to analyze the problem, present reasonable solutions, and get alignment on those solutions.

Start by creating a document [or use this template](https://docs.google.com/document/d/1BjBdeiZjPs5oHNkQzXBj8RNC5p2N20Kq54x9W3U4H2Q/edit?usp=sharing) and fill it in as you work through the following steps.
## Define and share the problem statement

Start with a clear problem statement, then use that to decide with your team whether the problem is worth solving now.
### Craft the problem statement
This is the single most important part of problem solving and the one I see done  wrong most frequently. Your problem should:

1. Be specific and time bound

    Define your problem as precisely as possible. What portion of your system is impacted, and how? If customers are experiencing issues, what are those issues? Try to keep the problem as tightly scoped as possible - "The whole system is a mess" is a harder problem to solve than "The Java style in this microservice is inconsistent".

2. Clearly state the importance of the problem

    If possible, explain why your problem is worth solving now. How is this issue impacting the business?

3. Not be tied to a specific solution

    Avoid including any of the proposed solutions in your problem statement. If the problem is tied to a technology, then we're excluding other possible solutions implicitly.

Defining a good problem statement is important because it allows you to measure success and it helps your organization align on a common goal. In the end, you may find that when you take the time to write a problem statement, you will discover that the problem is smaller than it initially appeared and isn't worth solving right now.

**Poor Problem Statement**

"We should be using Samza for our nearline processing because it's going to reduce the spikes in our push notification latency."

**Better Problem Statement**

"Our push notification pipeline was operating above our team's 2 second 95th percentile SLA every day last week."

**Best Problem Statement**

"Our push notification pipeline was operating about 1 second above our team's 2 second 95th percentile SLA for at least 10 minutes every day last week. Push notification latency is correlated with app engagement, which is a true north metric for our business."

### Explore and document the existing state of the problem

What is the root cause of the problem? What are the component systems? What do you need to document for your team and yourself so that everyone understands the details of the issue?

Document the current state, so that when folks are reviewing your document (now or in the future) they can quickly come up to speed on the details of the problem. When doing this documentation, consider who the document is going to be shared with and bring the documentation to the level of your audience. If you’re sharing with other engineers, including very specific details may be helpful, if you’re sharing with product managers, describing the problem in broad strokes may be more appropriate. Remember that you are a member of your audience as well, include the information you’d need to get back up to speed on the problem.

Consider the previous example of our push notification pipeline. The description of our problem state might include graphs showing the latency in both the stream processing system as well as the push notification decoration system. We could include a spreadsheet showing which push notifications types are impacted - “Messages are fine, but Feed update pushes are really slow”. We can add links to the specific implementations for each of these paths. If there are waterfall diagrams or examples of especially slow push notifications, we can include those.

### Share your problem statement

Sharing your problem helps to short-circuit two important pieces of feedback: “This problem is not worth solving” and “We already have a solution to a similar problem”. Send your problem statement and your understanding of that problem to members of your organization and ask them for feedback. I’ve found that this works best in the form of a Google Doc, so that your colleagues can comment and discuss amongst themselves. Your goal here is to get agreement that the problem is worth solving and that there doesn't exist an easy solution.

## Propose Solutions

Once you understand the problem, propose solutions. Ideally there are a variety of solutions that will address your problem.

For each proposal, describe the solution, why it addresses the problem, and the estimated cost of implementing it. Call out potential issues with each solution, these will help you to determine which solution among the many is best to pursue.

Then, select a single proposal you recommend proceeding with. This may change after discussion with the team, but presenting a specific approach in your first draft will jump start the discussion.

## Collaborate with your team to solve the problem

This is where the team part of "solving problems as a member of a team" comes in. We’ve previously agreed that the problem is worth solving, in this stage we are agreeing on a solution to attempt.

I typically will share my document with a technical teammate or two to sanity check my solutions before bringing in other stakeholders. Once your document is sanity checked it's important to share your document widely and explicitly request sign offs from stakeholders who may be affected by your problem or solution.

By sharing your document you give folks the opportunity to bring up issues which may have missed, endorse solutions, or present new solutions which haven't been considered. It's an excellent learning opportunity and can help to arrive at the best solution.These reviews can occasionally take a while, so depending on the urgency of the problem, arranging a meeting with the stakeholders can be a useful way to impose a deadline on a decision and sign off. It can also be helpful to have an explicit decision maker, who can help you break deadlock when there are multiple competing proposals. If there’s a manager or technical lead in your team, consider listing them as the decider and working with them to agree on a specific path forward.

### Implement your solution and test it
Once you’ve agreed on a solution, go ahead and implement it, deploy it, ramp it, and validate that it has solved the problem.

If the problem wasn’t solved, then it’s important to understand why it didn’t work and document it. It’s worth asking a few questions at this point: Has our understanding of the problem changed? Have the set of viable alternatives changed? Has our prioritization of the problem changed?

You have the opportunity to now iterate through the "Propose solutions" and "Share your document" steps and implement a new solution.
If our solution does work, then we can move on to the final step!

## Document the final state
Have you ever looked at a system and wondered "Why on earth did they do this?" I try to save these problem statement documents so that future me, and future teammates can make sense of the problem I was trying to solve when I built a particular system. I highly recommend creating your own organized way to store these for future reference.

## Conclusion
Hopefully you find this to be a helpful process you can follow to organize and respond to problems you encounter!

This process can scale up or down depending on the size of the problem. I've solved little problems this way using Slack threads, and have used more formal processes, like the RFC process for big, difficult to reverse decisions. Definitely don't feel like you need to pen yourself into a single format. But if you're feeling like it, then I've put together a short template which you can use to make this process easier.

If you use this and find it useful or if you have a novel problem solving process which blows this one out of the water, please let me know!