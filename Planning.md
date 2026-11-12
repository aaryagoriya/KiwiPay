# KiwiPay: Project Plan

**App Development Life Cycle (ADLC), Stage 1: Planning**

| | |
| :--- | :--- |
| **Project** | KiwiPay, smart shift tracking and payroll estimation for NZ hourly workers |
| **Document** | Project Plan (ADLC Stage 1) |
| **Version** | 1.0 (Draft) |
| **Author** | Aarya Goriya |
| **Date** | 12 November 2026 |
| **Project status** | Planning. No development work has started yet. |

---

## Contents

1. [Project Overview](#1-project-overview)
2. [Objectives and Success Criteria](#2-objectives-and-success-criteria)
3. [Scope](#3-scope)
4. [Users](#4-users)
5. [ADLC Approach](#5-adlc-approach)
6. [NZ Rules the App Must Follow](#6-nz-rules-the-app-must-follow)
7. [Requirements](#7-requirements)
8. [Architecture and Design](#8-architecture-and-design)
9. [Full Roadmap](#9-full-roadmap)
10. [Timeline](#10-timeline)
11. [Testing Strategy](#11-testing-strategy)
12. [Deployment Plan](#12-deployment-plan)
13. [Maintenance Plan](#13-maintenance-plan)
14. [Risk Register](#14-risk-register)
15. [Repository and Workflow](#15-repository-and-workflow)
16. [Open Decisions](#16-open-decisions)
17. [Decisions Log](#17-decisions-log)
18. [Sources](#18-sources)
19. [Disclaimer](#19-disclaimer)

---

## 1. Project Overview

KiwiPay is a cross-platform mobile app (iOS and Android) that helps hourly workers, casual employees and multi-job holders in New Zealand track their shifts and estimate their take-home pay. It combines shift logging with automatic calculation of PAYE tax (M and S codes), ACC Earners' Levy, KiwiSaver, public holiday pay and alternative leave (days in lieu).

**The problem it solves**

- Workers on irregular rosters find it hard to know whether a public holiday is an Otherwise Working Day (OWD).
- Working two jobs often causes tax surprises because of the wrong M or S tax code.
- Employees cannot easily check whether their payslip deductions are correct.
- Days in lieu earned from working public holidays are often lost or not recorded.

---

## 2. Objectives and Success Criteria

These are proposed targets. Adjust them if your goals change.

| # | Objective | Success criteria |
| :--- | :--- | :--- |
| O1 | Accurate pay estimates | Engine output matches IRD examples and a real payslip in every test case |
| O2 | Correct public holiday handling | 1.5x pay and alternative leave are applied correctly in all test scenarios |
| O3 | Private, secure data | A test user cannot read or change another user's data |
| O4 | Usable on a phone during a shift | A shift can be logged quickly, including without internet (Phase 2) |
| O5 | Maintainable rules | Tax and KiwiSaver rates live in one config file and can be updated without editing the engine logic |

---

## 3. Scope

**In scope**

- Multi-job profiles with hourly rates, tax codes, penal rates and break deductions
- Shift logging (past shifts) and rostering (upcoming shifts)
- PAYE for M and secondary tax codes, ACC Earners' Levy, KiwiSaver
- OWD detection, public holiday pay, alternative leave accrual
- Leave balances: sick, alternative, annual
- Offline use, student loan deduction, payslip scanning and CSV/PDF export (Phases 2 to 5)

**Out of scope**

- Employer payroll or IRD tax filing (KiwiPay is not certified payroll software)
- Countries other than New Zealand
- Legal or tax advice

---

## 4. Users

| User group | Need |
| :--- | :--- |
| Hourly and shift workers (retail, hospitality, healthcare support, logistics) | Know expected pay and check public holiday entitlements |
| Dual-job holders | Get the primary (M) and secondary (S) tax right |
| Casual and gig workers | Keep proof of work patterns to support holiday entitlements |
| Students and working holiday visa holders | Understand NZ tax codes and employment rights |

---

## 5. ADLC Approach

**Method:** iterative. Each roadmap phase goes through design, build, test and release before the next one starts. Small milestones keep the project manageable for one developer.

| Stage | Purpose | Main output | Status |
| :--- | :--- | :--- | :--- |
| 1. Planning | Define goals, scope, risks, roadmap | This document | In progress |
| 2. Requirements | Define exactly what the app must do | Section 7 (draft, to be confirmed) | Drafted |
| 3. Design | Plan architecture, data model, screens | Section 8, plus wireframes | Not started |
| 4. Development | Build the app milestone by milestone | Working code in GitHub | Not started |
| 5. Testing | Prove calculations and security are correct | Passing tests, test log | Not started |
| 6. Deployment | Release to testers, then users | Test builds, release | Not started |
| 7. Maintenance | Keep rules and code up to date | Yearly update routine | Not started |

---

## 6. NZ Rules the App Must Follow

These were checked against official and industry sources on 12 November 2026. The app should be built to these rules from the start. Note that the project README currently contains out-of-date KiwiSaver details and should be updated (see Milestone 1.1).

| Topic | Rule to implement | Notes |
| :--- | :--- | :--- |
| KiwiSaver employee rate | Default and minimum is 3.5% from 1 April 2026, rising to 4% from 1 April 2028. Higher options are 4%, 6%, 8%, 10%. A 3% rate is only possible with a temporary rate reduction. | The rate that applies is based on the pay date, not the pay period. Make the rate date-based. |
| KiwiSaver employer contribution | Minimum 3.5% from 1 April 2026, 4% from 1 April 2028. Compulsory for eligible 16 and 17 year olds from 1 April 2026. | Employer contributions are on top of the employee's pay. |
| Public holiday worked | At least time-and-a-half for hours worked. An alternative holiday (day in lieu) is earned only if the day is an Otherwise Working Day. | Matches README logic. |
| Otherwise Working Day (OWD) | Under the Holidays Act 2003, this is a judgement based on the employment agreement, work pattern and roster. No fixed formula was found in Employment NZ guidance. | Present the "3 in 5 weeks" check in the app as KiwiPay's estimate, not the legal test. |
| Holiday law change | The Employment Leave Act 2026 received Royal Assent on 6 August 2026 and replaces the Holidays Act 2003 on 6 August 2028. Leave becomes hours-based. The Holidays Act still applies until then. | Build to the Holidays Act now. Keep leave logic separate so it can be replaced in 2028. |
| New OWD test (from 2028) | The bill as introduced treats a day as an OWD if the employee worked (or was on leave) on 50% or more of the same weekday in the preceding 13 weeks, for employees with no set days in their agreement. | Re-check the final Act text before 2028. |
| PAYE tables, ACC levy rate and cap, student loan threshold | Not verified in this plan. | Look up current values on the IRD website when building Milestones 1.3, 1.4 and Phase 3. Put them in the config file. |
| Sick leave | README states 10 days after 6 months of continuous employment. | Confirm on Employment NZ when building the leave vault. |

The NZ tax year runs from 1 April to 31 March, so rates usually change on 1 April.

---

## 7. Requirements

Priority uses MoSCoW: **M**ust, **S**hould, **C**ould. Priorities are proposed and can be changed.

### 7.1 Functional requirements

| ID | Requirement | Priority | Phase |
| :--- | :--- | :--- | :--- |
| FR-01 | Sign up, log in and log out with email and password | M | 1 |
| FR-02 | Create and edit multiple jobs (title, hourly rate, tax code) | M | 1 |
| FR-03 | Log past shifts (date, start, end, break) | M | 1 |
| FR-04 | Custom night and weekend penal rates per job | S | 1 |
| FR-05 | Schedule upcoming shifts with projected gross and net pay | S | 1 |
| FR-06 | Calculate PAYE for M and secondary tax codes | M | 1 |
| FR-07 | Calculate ACC Earners' Levy | M | 1 |
| FR-08 | Select KiwiSaver rate and calculate the deduction; optional employer contribution tracking | M | 1 |
| FR-09 | Detect whether a public holiday is an OWD from past shifts | M | 1 |
| FR-10 | Apply 1.5x pay to hours worked on a public holiday | M | 1 |
| FR-11 | Add 1 alternative leave day when an OWD public holiday is worked | M | 1 |
| FR-12 | Show and update sick, alternative and annual leave balances; log leave taken | M | 1 |
| FR-13 | Weekly income dashboard across all jobs | M | 1 |
| FR-14 | Log shifts offline and sync when online | S | 2 |
| FR-15 | Student loan repayment deduction | S | 3 |
| FR-16 | Scan a payslip and compare it with KiwiPay's calculation | C | 4 |
| FR-17 | Export shift logs and pay summaries to CSV and PDF | S | 5 |
| FR-18 | Magic link login and biometric session recovery (listed in the README Auth feature) | C | After 1 |

### 7.2 Non-functional requirements

| ID | Requirement |
| :--- | :--- |
| NFR-01 | **Accuracy:** all calculations are covered by unit tests and checked against IRD sources |
| NFR-02 | **Security:** row level security so each user only accesses their own data |
| NFR-03 | **Privacy:** pay data is never written to logs; API keys are never committed to GitHub |
| NFR-04 | **Maintainability:** rates and thresholds are in a config file, and the calculation engine has no UI or database code in it |
| NFR-05 | **Compatibility:** works on current iOS and Android |
| NFR-06 | **Usability:** clear screens that a first-time NZ worker can use without instructions |

---

## 8. Architecture and Design

### 8.1 Technology stack

| Layer | Technology | Purpose |
| :--- | :--- | :--- |
| Mobile app | React Native with Expo Router, TypeScript | iOS and Android from one codebase |
| Styling | NativeWind | Utility-style styling |
| Backend and database | Supabase (PostgreSQL) | Users, jobs, shifts, leave balances |
| Authentication | Supabase Auth | Login and sessions |
| Security | PostgreSQL row level security (RLS) | Per-user data access |
| Offline state | Zustand and MMKV | Local storage and syncing (Phase 2) |
| Calculation engine | Pure TypeScript module | Tax and Holidays Act logic, testable on its own |

### 8.2 Data model

Planned tables: `profiles`, `jobs`, `shifts`, `leave_balances` (SQL is in the README).

### 8.3 Design items to resolve in Stage 3

Reading the README schema and feature list shows these gaps. Resolve each before building the related milestone.

1. **RLS policies:** the README only enables RLS. With RLS on and no policies, normal users cannot read or write anything. Policies are needed on every table (for example `auth.uid() = user_id`, and `auth.uid() = id` on `profiles`).
2. **Penal rates and overtime:** these appear in the features and the pay formula but have no columns in the schema.
3. **Sick leave default:** `sick_leave_days` defaults to 10, but sick leave is earned after 6 months. A starting value of 10 may overstate it.
4. **Leave units:** alternative leave is in days and annual leave is in hours. The 2028 law is hours-based, so choose one unit.
5. **Public holiday dates:** no source is defined. Some dates, such as Matariki, change each year.
6. **Stored results:** `gross_pay`, `is_owd` and `is_public_holiday` are saved on each shift. Decide whether to also save the rate used, so old shifts stay explainable after rates change.
7. **Tax profiles:** the README says the database stores tax profiles, but there is no such table. The tax code is currently on `jobs`. Student loan (Phase 3) will also need a place to be stored.

### 8.4 Core user flow

Shift entry, then select job (tax code), then public holiday check, then OWD check (if a holiday), then pay and deductions, then sync to Supabase, then dashboard with net pay and leave.

---

## 9. Full Roadmap

Nothing has been built yet, so every item below is **not started**. Estimates are in Section 10.

### Phase 1: Core App (MVP)

**Goal:** a working app that logs shifts for one or more jobs and calculates net pay, public holiday pay and leave correctly.

#### Milestone 1.1: Project setup
- [ ] Create the GitHub repository and add the README and this plan
- [ ] Update the README with the current KiwiSaver rates and the OWD wording from Section 6
- [ ] Create the Expo project with Expo Router and TypeScript
- [ ] Add NativeWind and check styling works
- [ ] Set up a test runner for the engine (for example Jest) and basic linting
- [ ] Create the folder structure (Section 15)

**Done when:** the starter app runs on a phone or emulator and the repository is on GitHub.

#### Milestone 1.2: Backend and security
- [ ] Create a Supabase project for development
- [ ] Create the tables: `profiles`, `jobs`, `shifts`, `leave_balances`
- [ ] Apply the design decisions from Section 8.3
- [ ] Enable RLS and write policies for every table
- [ ] Configure email and password authentication
- [ ] Connect the app using environment variables (no keys in GitHub)

**Done when:** two test users can sign up and neither can see the other's data.

#### Milestone 1.3: PAYE engine
- [ ] Look up current PAYE rates on the IRD website and store them in the config file
- [ ] Implement tax code M
- [ ] Implement secondary codes SB, S, SH, ST (decide whether SA is included, see Section 16)
- [ ] Handle two concurrent jobs (one M, one secondary)
- [ ] Write unit tests, including income at the edge of each bracket

**Done when:** results match IRD examples for each tax code.

#### Milestone 1.4: ACC levy, KiwiSaver and net pay
- [ ] Add the ACC Earners' Levy rate and earnings cap from IRD to the config file
- [ ] Implement KiwiSaver employee rates (3.5%, 4%, 6%, 8%, 10%) chosen by pay date
- [ ] Add optional employer contribution tracking (3.5% from 1 April 2026, 4% from 1 April 2028)
- [ ] Implement net pay: gross minus PAYE, ACC levy and KiwiSaver
- [ ] Write unit tests, including pay dates before and after 1 April 2026

**Done when:** a sample week's net pay matches a hand calculation and a real payslip.

#### Milestone 1.5: OWD engine
- [ ] Write a short specification for the "3 in 5 weeks" check, including how weeks with leave are treated
- [ ] Implement it as a pure function using the past 5 weeks of shifts
- [ ] Write unit tests: exactly 3 of 5, 2 of 5, no history, a gap week
- [ ] Label the result as an estimate wherever it is shown

**Done when:** all test cases pass.

#### Milestone 1.6: Public holiday pay and alternative leave
- [ ] Choose a source for public holiday dates (Section 16)
- [ ] Apply 1.5x pay to hours worked on a public holiday
- [ ] Add +1 alternative leave day when an OWD public holiday is worked
- [ ] Show the paid-day-off result when an OWD public holiday is not worked
- [ ] Write unit tests for worked OWD, worked non-OWD and not worked

**Done when:** all three scenarios give the correct pay and leave result.

#### Milestone 1.7: App screens
- [ ] Sign up, log in and log out screens
- [ ] Add and edit job screens (rate, tax code, penal rates)
- [ ] Shift logging for past shifts and scheduling for upcoming shifts
- [ ] Weekly income dashboard across jobs
- [ ] Leave screens: balances and log leave taken
- [ ] Settings: KiwiSaver rate and employer contribution toggle

These screens are described in the README features but are not in the README roadmap checklist, so they are added here.

**Done when:** a user can complete the full flow in Section 8.4 on a phone.

#### Milestone 1.8: Phase 1 testing and release
- [ ] Run the full test checklist (Section 11)
- [ ] Compare outputs with IRD calculators and a real payslip
- [ ] Fix all calculation bugs
- [ ] Tag the release in Git (for example `v0.1.0`)
- [ ] Update the README and this plan

**Phase 1 exit criteria:** all FR marked Phase 1 (Must) work, all tests pass, RLS is verified.

---

### Phase 2: Offline-First Caching

**Goal:** log shifts without internet and sync to Supabase when the connection returns.

- [ ] Set up the Zustand store with MMKV persistence
- [ ] Build a queue of unsynced shifts
- [ ] Upload the queue automatically when the network returns
- [ ] Choose a simple conflict rule (for example, latest edit wins)
- [ ] Test in airplane mode: no lost or duplicated shifts
- [ ] Note: MMKV uses native code, so it will likely not run in Expo Go and needs a development build. Check the react-native-mmkv docs when starting.

**Exit criteria:** FR-14 works and offline tests pass.

---

### Phase 3: Student Loan Repayment

**Goal:** include the student loan deduction in net pay.

- [ ] Add the SL code to the tax profile
- [ ] Look up the current repayment threshold and rate on the IRD website and add them to the config file
- [ ] Include the deduction in the net pay calculation
- [ ] Write unit tests and compare with a worked IRD example

**Exit criteria:** FR-15 works and results match IRD guidance.

---

### Phase 4: Payslip PDF Scanner / OCR

**Goal:** cross-check an employer payslip against KiwiPay's calculation.

- [ ] Build a small prototype using 3 to 5 sample payslips
- [ ] Decide where scanning runs (on the phone or on a server)
- [ ] Extract key fields: gross pay, PAYE, KiwiSaver, net pay
- [ ] Show differences between the payslip and KiwiPay's result
- [ ] Decide how scanned payslips are stored and deleted
- [ ] Test with different payslip layouts

**Exit criteria:** FR-16 works on the sample payslips, and privacy handling is documented.

---

### Phase 5: Export

**Goal:** export shift logs and pay summaries.

- [ ] CSV export of shifts
- [ ] PDF weekly pay summary
- [ ] Test that exported files open correctly on a computer and show correct totals

**Exit criteria:** FR-17 works.

---

## 10. Timeline

Durations are initial estimates, assuming part-time work alongside study. Review them after Milestone 1.2 and adjust. Dates are not fixed until you choose a start date.

| Item | Estimated duration | Depends on |
| :--- | :--- | :--- |
| 1.1 Project setup | 1 week | None |
| 1.2 Backend and security | 1 to 2 weeks | 1.1 |
| 1.3 PAYE engine | 2 weeks | 1.1 |
| 1.4 ACC, KiwiSaver, net pay | 1 to 2 weeks | 1.3 |
| 1.5 OWD engine | 1 week | 1.1 |
| 1.6 Public holiday and alternative leave | 1 to 2 weeks | 1.4, 1.5 |
| 1.7 App screens | 3 to 4 weeks | 1.2, 1.6 |
| 1.8 Testing and release | 1 week | 1.7 |
| **Phase 1 total** | **about 11 to 15 weeks** | |
| Phase 2 | 3 to 4 weeks | Phase 1 |
| Phase 3 | 1 to 2 weeks | Phase 1 |
| Phase 4 | 4 to 6 weeks | Phase 1 |
| Phase 5 | 2 weeks | Phase 1 |

Milestones 1.3, 1.5 and 1.2 can run in parallel. Phases 3, 4 and 5 can be done in any order after Phase 1.

---

## 11. Testing Strategy

### 11.1 Unit tests (calculation engine)

| Area | Cases |
| :--- | :--- |
| OWD | Exactly 3 of 5 weeks, 2 of 5 weeks, no history, a gap week |
| Public holiday pay | Worked OWD (1.5x and +1 day), worked non-OWD (1.5x only), not worked |
| PAYE | Edge of each bracket, M vs secondary codes, two jobs |
| ACC | Below and above the earnings cap |
| KiwiSaver | Every rate option, pay dates before and after 1 April 2026 |
| Shifts | Crossing midnight, unpaid break removed |

### 11.2 Other testing

| Type | What to check |
| :--- | :--- |
| Security | User A cannot read or change user B's data |
| Offline (Phase 2) | No lost or duplicate shifts after reconnecting |
| Accuracy | Compare with IRD calculators and a real payslip |
| Devices | At least one Android and one iOS device or simulator |

### 11.3 Definition of done (every milestone)

- [ ] All tasks in the milestone are finished
- [ ] New logic has unit tests and all tests pass
- [ ] Results are checked against an official or real-payslip source
- [ ] Code is committed to a feature branch and merged to `main`
- [ ] README and this plan are updated

---

## 12. Deployment Plan

| Step | Detail | Status |
| :--- | :--- | :--- |
| Environments | Separate Supabase projects for development and production | Not started |
| Secrets | Keys stored in environment variables, never in GitHub | Not started |
| Test builds | Internal builds for a small group of testers | Not started |
| Store release | Decide later whether to publish to the app stores | Not decided |

You cannot build iOS apps locally on Windows. Expo's cloud build service (EAS Build) can build iOS apps for you. Check the Expo documentation for current setup steps when you reach this stage.

---

## 13. Maintenance Plan

| When | Task |
| :--- | :--- |
| Every 1 April | Check PAYE tables, ACC levy rate and cap, KiwiSaver rates, student loan threshold. Update the config file and tests. |
| Every year | Update public holiday dates |
| Before 6 August 2028 | Read the final Employment Leave Act 2026 and Employment NZ guidance. Update the OWD test, move leave to hours and plan how existing balances convert. |
| Ongoing | Keep a bug list and fix wrong calculations first |
| Ongoing | Update Expo and other packages one at a time and re-run the tests |

---

## 14. Risk Register

Likelihood (L) and impact (I) are rated Low, Medium or High.

| ID | Risk | L | I | Mitigation |
| :--- | :--- | :--- | :--- | :--- |
| R1 | Tax, ACC or KiwiSaver rules change | High | High | Single config file, yearly review each 1 April |
| R2 | Holidays Act replacement changes leave logic in 2028 | High | Medium | Keep leave logic in its own module |
| R3 | OWD estimate is wrong for some work patterns | Medium | High | Label as an estimate, test edge cases, let the user override |
| R4 | Users treat estimates as official payroll | Medium | Medium | Visible disclaimer in the app and README |
| R5 | Missing or wrong RLS policies expose data | Low | High | Write policies in 1.2 and test with two users |
| R6 | OCR (Phase 4) is harder than expected | High | Medium | Prototype first and do Phase 4 last |
| R7 | Time pressure from study | High | Medium | Small milestones, review estimates after 1.2 |
| R8 | Sensitive pay data leaks through logs or stored files | Low | High | No pay data in logs, clear storage and deletion rules |

---

## 15. Repository and Workflow

**Suggested structure** (adjust as needed):

```
kiwipay/
  app/          Expo Router screens
  src/engine/   Calculation engine (PAYE, OWD, holiday pay, KiwiSaver)
  src/config/   Rates and thresholds
  supabase/     SQL for tables and RLS policies
  docs/         Plans and notes
  README.md
  planning.md
```

**Git workflow**

- `main` always contains working code
- One branch per milestone or fix, for example `feature/paye-engine` or `fix/kiwisaver-rate`
- Small commits with clear messages, for example `Add PAYE tax code M`
- Merge into `main` only after the tests pass

---

## 16. Open Decisions

| # | Decision | Needed by |
| :--- | :--- | :--- |
| D1 | Is secondary tax code SA included? (Listed in the README features but not in the roadmap or schema.) | 1.3 |
| D2 | Support the temporary 3% KiwiSaver rate reduction? | 1.4 |
| D3 | Show employer KiwiSaver contributions before or after employer tax on the contribution (ESCT)? | 1.4 |
| D4 | Leave units: days or hours? | 1.2 |
| D5 | Source of public holiday dates | 1.6 |
| D6 | Save the rate used on each shift? | 1.2 |
| D7 | Offline conflict rule | Phase 2 |
| D8 | Where OCR runs | Phase 4 |
| D9 | How testers get builds, and whether to publish to stores | Before first release |
| D10 | Project start date and weekly time available | Before Milestone 1.1 |

---

## 17. Decisions Log

| Date | Decision | Reason |
| :--- | :--- | :--- |
| 12 Nov 2026 | Use an iterative approach, one roadmap phase at a time | One developer, small milestones |
| 12 Nov 2026 | Build to the 2026 KiwiSaver rates (3.5%, rising to 4% in 2028) from the start | README rates are out of date |
| 12 Nov 2026 | Treat "3 in 5 weeks" as an estimate, not the legal test | Not confirmed as an official rule |
| 12 Nov 2026 | Add Milestone 1.7 (screens) to the roadmap | App screens are in the README features but not in its roadmap |

---

## 18. Sources

- IRD, KiwiSaver changes: https://www.ird.govt.nz/kiwisaver-changes
- Employment NZ, Employment Leave Bill 2026: https://employment.govt.nz/news-and-updates/employment-leave-bill-2026
- Employment NZ, Otherwise working day: https://employment.govt.nz/leave-and-holidays/otherwise-working-day
- NZ Legislation, Employment Leave Bill: https://www.legislation.govt.nz/bill/government/2026/259/en/latest
- Inland Revenue (IRD) website: PAYE tables, ACC levy and student loan rates (look up current values at build time)

---

## 19. Disclaimer

KiwiPay is an independent estimation and shift-tracking tool. It is not certified IRD payroll software and is not legal or tax advice. Always check rates and rules against IRD and Employment New Zealand.

---

**Document history**

| Version | Date | Change |
| :--- | :--- | :--- |
| 1.0 | 12 Nov 2026 | First draft (ADLC Stage 1: Planning) |