---
layout: post
title: "TFT App: Natural Language Querying for Game Data"
date: 2026-05-06
---

# TFT App: Natural Language Querying for Game Data

## Project Overview

This project is about designing and building a TFT data-querying application that helps players ask complex questions about Teamfight Tactics gameplay data without needing to write SQL manually.

As an advanced TFT player, I often had specific questions about the game that were difficult to answer using existing statistics websites. For a technical user, this type of analysis can be done by collecting match data, storing it in a database, and writing SQL queries. However, this is not accessible for many non-technical TFT players.

Most existing TFT tools support common filters and standard statistics, but they are limited when users want to ask more complex questions involving specific units, items, team compositions, or logical conditions.

For example, a user may want to ask:

> What is the win rate of Jinx 3-star with Infinity Edge, but without Kai'Sa on the team, and with Last Whisper?

This type of question is difficult to express using normal filter-based websites. Therefore, the goal of this project is to make advanced TFT data analysis easier, more flexible, and more accessible through natural language interaction.

---

## Project Goal

The goal of the TFT App is to allow users to ask complex TFT-related questions in natural language and receive understandable, query-based results.

The project aims to:

- Help non-technical TFT players access advanced game data.
- Support flexible queries that go beyond standard statistics websites.
- Reduce the need for users to understand SQL.
- Make AI-generated query results feel more transparent and trustworthy.
- Give users a sense of control over how their question is interpreted.
- Create a simple interface based on HCI usability principles.

The main idea is not only to convert natural language into SQL, but also to first rephrase what the system understood. This allows the user to confirm whether the system interpreted their question correctly before trusting the result.

---

## Problem Motivation

The core problem is that advanced game analysis is often locked behind technical knowledge.

TFT players may want to ask very detailed questions about combinations of units, items, traits, and team compositions. However, most tools only allow users to choose from predefined filters. This works for simple statistics, but it does not support more complex logic such as `NOT`, `AND`, `OR`, or exclusion-based conditions.

A full SQL interface could solve this problem, but it would be too intimidating for many players. A non-technical user may not know how to write queries, understand database structure, or debug errors.

This creates an HCI design challenge:

> How can a system provide the power of SQL while keeping the interaction simple enough for ordinary TFT players?

The solution I explored is a natural language interface supported by a structured database query system.

---

## Learning and Execution Process

## 1. Understanding the User Need

The project started from my own experience as a TFT player. I noticed that I wanted to ask more advanced questions than what existing tools allowed.

At the same time, I realized that my technical background made the problem easier for me than for other users. I could call an API, collect match data, use PostgreSQL, and write my own queries. However, most players would not want to interact with the data this way.

This helped me define the target users:

- TFT players who want deeper gameplay insights.
- Users who understand the game but do not know SQL.
- Advanced players who want more flexible statistics.
- Casual users who may prefer simple natural language input.

The main user need is flexibility without technical complexity.

---

## 2. Initial Technical Direction

The first version of the project was based on a database-backed system. The idea was to collect TFT match data and allow users to query it.

At first, I considered using Google Cloud Platform and BigQuery. However, persistent storage costs became a concern because this project requires a database that users can consistently query. As a result, I decided to move toward a more economical infrastructure setup.

The system was moved to a Linux home server running on an old laptop. This made the project cheaper to maintain while still being sufficient for the expected traffic.

The infrastructure included:

- A Debian-based home server.
- Cloudflare tunneling for public access.
- PostgreSQL for persistent data storage.
- Docker for containerized deployment.
- GitHub Actions for CI/CD.

This setup allowed the project to stay practical, portable, and low-cost.

---

## 3. PostgreSQL Query System

PostgreSQL was used as the main database system.

Instead of directly exposing SQL to the user, the app uses an abstracted querying system. This is important because users should not need to know database syntax. It also helps keep inputs controlled and sanitized.

The database layer supports the main technical purpose of the app: answering complex TFT questions using real match data rather than generating unsupported AI guesses.

This is important for trust. The app should not simply give users an AI-written answer. It should use AI to help translate the question, but the actual result should come from a structured query over data.

---

## 4. Natural Language to SQL

The second major feature was Natural Language to SQL, or NL2SQL.

The idea is that users can describe their question in normal language, and the system translates it into a database query.

However, testing the prototype showed a major issue: users did not fully trust direct NL2SQL output. Some users felt that the answer might be hallucinated by AI, even when a proper database query was being used.

This became an important HCI insight. The issue was not only whether the system was technically correct. The issue was whether users felt that they understood and controlled the process.

To address this, I added an interpretation step. Before returning a result, the system explains what it understood from the user.

For example, the app may respond with:

> I understand that to mean: find the win rate of games where Jinx is 3-star, Jinx has Infinity Edge, the team does not include Kai'Sa, and the team includes Last Whisper.

This gives the user a chance to check the interpretation. It also makes the AI feel less like a black box.

---

## 5. User Interface Design

After the infrastructure and querying system were working, I focused on the user interface.

My goal was to maximize usability. Since the target users may not be technical, the app needed to feel simple and approachable. At the same time, it should still be powerful enough for advanced users.

The interface was designed around several HCI principles.

### Consistency

The app uses familiar website conventions, such as a clear input area, output area, and run button. The run button is visually distinct so users understand the main action.

The output box is separated from the input area, making it clear where users type and where results appear.

### Minimalism

The interface avoids unnecessary technical details on the main screen. This reduces intimidation for first-time users.

More advanced information is hidden under a section such as "Syntax for Nerds", so technical users can access extra details without overwhelming beginners.

### User Freedom

Users can ask questions in natural language, but more advanced users can still use structured syntax if they want more control.

This makes the app usable for both beginners and power users.

### Visibility of System Status

The app provides feedback while the system is processing. This helps users understand that the system is working instead of frozen.

Clear system feedback is especially important when using AI, because users may otherwise be unsure what is happening in the background.

### Recognition Rather Than Recall

The interface uses clear input and output boxes, visible actions, and simple labels. Users do not need to memorize commands before using the app.

This supports learnability and makes the system easier to return to after a break.

### Help and Recovery

The system guides users when the AI interpretation is not what they intended. This is important because natural language input can be ambiguous.

Instead of treating the AI output as final, the app allows the user to notice mistakes and revise the query.

---

## Usability Analysis

The app can be evaluated through three major usability qualities: learnability, efficiency, and memorability.

### Learnability

The app is designed to be easy to learn because users can type questions naturally. TFT players do not need database knowledge to start using it.

The interface also avoids presenting too much technical information at once, which helps reduce the learning barrier.

### Efficiency

The app is efficient because users do not need to go through a long tutorial or manually adjust many filters. They can directly ask the question they care about.

For advanced users, syntax-based querying can also make repeated use faster.

### Memorability

The app uses a consistent and minimal design, so users can quickly understand it again when they return.

Because the interaction flow is simple — enter question, check interpretation, run query, read result — the system should be easy to remember.

---

## My Responsibility in the Group

My responsibility in this project was to help connect the technical implementation with the HCI design direction.

My work included:

- Identifying the problem and user need.
- Defining the purpose of the app.
- Designing the natural language query interaction.
- Considering how users would trust or distrust AI-generated output.
- Helping shape the interface around usability principles.
- Thinking about transparency, control, and feedback.
- Supporting the project presentation and explanation.
- Reflecting on the limitations of the current prototype.

The main contribution was not only technical. It was also about designing the system so that users feel they understand what the app is doing.

---
