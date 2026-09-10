# Helm AI — config

Edit this file directly, or run `/setup <section>` to redo one section
(sections: me, boss, team, jira, products, extras, connectors, routines).
Every Helm command reads this first. Keep values on one line after the colon.

## Me
- name: 
- role: 
- company: 
- email: 
- timezone: America/New_York

## Boss
- name: 
- title: 
<!-- Anything from this person is 🔴 by default. -->

## Team
- direct reports: 
- close peers: 
<!-- Comma-separated first names as they appear in Slack/Jira. -->

## Jira
- projects: 
- support project: 
<!-- e.g. projects: CRM, ICD, DAI · support project: SUPPORT -->

## Products
- 

## Morning extras
- verse: off
- verse translation: ESV
- sports: off
- teams:
  - 
  - 
<!-- verse: on|off. sports: on|off. Max two teams, e.g. "🐬 Miami Dolphins (NFL)". -->

## Routines
- slack briefs: off
<!-- on = 1pm check-in and 5pm end-of-day delivered to your Slack DM by a cloud routine. -->
