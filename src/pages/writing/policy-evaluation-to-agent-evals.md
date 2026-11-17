---
layout: ../../layouts/Post.astro
title: What evaluating public policy taught me about evaluating AI agents
description: Five lessons from a decade-old discipline (rubrics, rater agreement, incentives, attribution and continuous evaluation) applied to AI agents in production.
date: 2026-11-17
readingTime: 6 min read
---

Between 2010 and 2015 I worked at a university research lab evaluating public science and technology policy in Brazil. One study asked a simple question about the country's main fiscal incentive for the ICT industry: did the tax breaks make the sector more competitive? The answer was mostly no. Local production grew, but value added stayed low and exports barely moved.

Today I lead an AI platform where many teams ship skills into a single customer-facing agent, and most of my week goes into one question: is this agent actually doing what we think it does?

The two questions are closer than they look. Both are about judging a complex system whose behaviour you cannot read directly, using imperfect measurements and people who disagree. Here are five lessons from policy evaluation that I now apply to AI agents.

## 1. Define success before you measure

A policy evaluation starts with a logic model: what the program is supposed to change, through which mechanism, and how you would recognise that change. Skip that step and you end up measuring whatever is easy to count. In the ICT study, "more companies investing in R&D" looked like success until we asked whether that investment raised the sector's technological density. It mostly did not.

Agent teams fall into the same trap. They track response time, thumbs-up rates or how often the agent answers instead of handing off, and call it quality. Before building any eval, I now write down what a correct outcome looks like for each skill: when it should fire, when it should not, and what a good answer contains. That written rubric is the logic model. Every metric that comes later, from precision and recall on skill activation to judge scores, is only as good as that definition.

## 2. Agreement before automation

In program evaluation, when several specialists score the same project, the first thing you check is whether they agree with each other. If two experts read the same report and reach opposite conclusions, the problem is the rubric, not the experts, and no amount of extra data will fix it.

LLM-as-a-judge needs exactly this discipline. Before letting a model grade production conversations, humans and the judge label the same sample and we measure agreement with Cohen's kappa, not raw percentage, because with imbalanced classes a judge that always says "fine" can agree 90% of the time and be useless. We also measure agreement between two humans. That number is the ceiling: if people only reach a kappa of 0.7, asking the judge for 0.9 makes no sense. And we break agreement down per skill, because a judge that looks fine on average often fails on the rare skills that matter most.

## 3. Evaluators have incentives

Public programs are often evaluated with data supplied by the people who run them, and that data tends to show success. Good evaluation designs separate who executes from who measures.

The agent version: if the team that builds a skill also writes its test cases, the tests get easy. Nobody does this on purpose; you simply write the examples you already had in mind. In our platform, the evaluation dataset for skill activation is owned by the platform team, not by the skill owner. It mixes human-calibrated reference answers with synthetic conversations, and it is refreshed with real production failures, so a new skill is tested against messy, ambiguous inputs and against its neighbours, not only against its own happy path.

## 4. Attribution is hard

The hardest question in policy evaluation is not "did the indicator go up?" but "did it go up because of the policy?". Exports can rise because of the exchange rate, not the incentive. Without a counterfactual, you tell a story, not a result.

Agents have the same problem. A model upgrade, a prompt change and a new skill can ship in the same week, and the resolution rate moves. Which one did it? We answer that with two tools borrowed from evaluation practice. Offline, every release runs against the same fixed dataset, so changes are compared on identical inputs. Online, prompts and agent policies go through A/B or multi-armed bandit tests before they reach everyone. And when a new skill ships, we read the confusion matrix across all skills: a new skill with great recall can simply be taking traffic from an existing one, which looks like success until you check its neighbours.

## 5. Continuous evaluation beats one-off assessment

The main recommendation of our 2012 paper on Brazil's ICT policy, published in the *Revista Brasileira de Inovação*, was not a new incentive. It was a system of continuous evaluation built on indicators, because a single study every few years tells you what already went wrong, too late to change it.

Agents drift faster than policies. Users change how they write, new skills arrive, model versions change underneath you, and a judge that was calibrated last month can quietly lose its alignment with human reviewers. So evaluation cannot be a launch checklist. We are building it to run at three moments: an offline gate before every release, an LLM judge on production traffic, and a fixed human-labelled anchor set that is re-scored on a schedule and after every relevant change, to catch the judge itself drifting. Failures found in production go back into the offline dataset, which closes the loop.

## The same discipline, a faster system

None of these ideas is new. Evaluation researchers have argued about rubrics, rater agreement, incentives, attribution and monitoring for decades. What is new is the speed: a policy changes every few years, an agent can change every day. That makes the old discipline more necessary, not less.

In the next article I will go deeper into lesson 2: how to calibrate an LLM judge against human reviewers, step by step, with a small public tool you can try yourself.

If you work on agent evaluation, I'd like to hear how you handle the judge-drift problem. Which part of this matches what you see, and which part doesn't?
