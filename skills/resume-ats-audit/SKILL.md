---
name: "resume-ats-audit"
description: Audit a resume/CV against a specific job posting the way a talent recruiter and ATS system would — keyword/skill coverage, title and seniority alignment, experience/education match, and parsing-risk checks — producing a calibrated match score with a prioritized improvement plan to reach 90–98%. Uses a stored default resume so the user only needs to provide a job posting. Trigger this whenever the user shares a job posting/description/URL and asks for a review, audit, score, match check, ATS check, or "how well do I match this role" — and for follow-ups like "get me to 90%," "close the gap," "re-score it," or "update my resume." Also trigger when the user pastes a job URL without further instructions — they almost certainly want an audit.
---

# Resume ATS Audit

Act as a senior talent recruiter running a two-pass screen: first as an ATS parser (mechanical, keyword-driven), then as a human recruiter reviewing what got through. Real ATS systems (Workday, Greenhouse, iCIMS, Taleo, LinkedIn Recruiter) don't share one algorithm — present scores as calibrated ranges, never false-precision single numbers.

---

## Default Resume

When the user has not provided a resume in this conversation, use the following as the default. The user can update it at any time by saying "update my resume" or uploading a new file.

```
Efe Jessye Osawaru Oyelade
Senior Software Engineer
Stockholm, Sweden
eosawaruoyelade@gmail.com
LinkedIn: https://linkedin.com/in/efe-osa-osawaru-oyelade
GitHub: https://github.com/efe-osa

SUMMARY
Product-focused Software Engineer with experience designing and building reliable, consumer and SaaS products across frontend and full-stack environments. Experienced in owning features end-to-end, shaping component-driven architectures, troubleshooting complex problems, and collaborating with cross-functional teams to turn business and user needs into effective software. Strong expertise in React, TypeScript, Next.js, Node.js and .NET, with a priority on building accessible, performant, and scalable web applications.

With a background in Business Administration and Strategic Information Systems, I enjoy being involved beyond the code and want to grow my impact through product ownership, technical leadership, and closer collaboration with users and the business.

SKILLS

Languages: JavaScript, TypeScript, C#, SQL, Python, Semantic HTML5/CSS3

Frontend Development: React, Next.js, Redux, TanStack, ASP.NET Core, Tailwind, Bootstrap, Web Components, SCSS, Design Systems, Figma, Sketch

Backend Systems, Database & Cloud: Node.js, Express.js, REST, HTTP, Microservices, C#, .NET, GraphQL, Postgres, MySQL, AWS

Process and Quality: Azure, Docker, Sentry, Grafana, GitHub Copilot, Git, CI/CD pipelines, Testing (React Testing Library, xUnit, Jest, Cypress, Playwright), code review, Vite, Webpack, ESLint, Prettier

EXPERIENCE

Senior JavaScript Engineer | LeoVegas Group | MGM, Stockholm
November 2025 – February 2026
- Designed and implemented a cross-browser geolocation permission measurement system in collaboration with Data Analytics, defining and capturing permission status to support regulatory compliance for Canadian markets.
- Accelerated testing using GitHub Copilot and Backoffice-generated data, boosting geolocation system test coverage to 80% using Jest and RTL.
Tools: Next.js, TypeScript, GraphQL, Jest, React Testing Library (RTL), Turbo, Grafana, Launch Darkly, Google Tag Manager

Software Engineer | Ringtail Interactive (Now Brink Gaming), Stockholm
January 2023 – January 2025
- Led and managed the Salesforce integration epic with customer journey teams, providing a knowledge base for game developers and ease in customer support communication and experience.
- Designed and shipped the deployment management module on the customer portal, enabling multiplayer game developers to automate and manually launch code releases.
- Architected and developed in-house WCAG-aligned responsive UI component library adopted across 90% of the customer portal, standardizing UI consistency, promoting company branding, and accelerating frontend velocity.
- Implemented a predictable state management pipeline using Fluxor (Flux architecture); integrated Azure Key Vault, Storage, and AD B2C to secure enterprise secrets, cloud assets, and identity federation.
Tools: C#, ASP.NET Core, Blazor, Fluxor, Bootstrap, Postgres, EF Core, SQL, SCSS, xUnit, Azure, Docker

Senior Frontend Engineer | Reliance HMO, Abuja
October 2021 – June 2022
- Owned and migrated a legacy platform to a component-based JavaScript architecture, scaling performance for 250,000+ active users and 4,000+ businesses within a scrum environment.
- Built an intuitive WYSIWYG contract review and signing tool using CKEditor, increasing user data accuracy by 25% during provider acquisition processes.
- Collaborated with a cross-functional agile team to troubleshoot code issues and redesign the UX of the audit system & providers' acquisition tracker, directly contributing to product maturity & a successful $40 million Series B funding round.
- Translated non-technical stakeholders' feedback into product improvements, ensuring 100% alignment with design specifications and optimizing user adoption.
Tools: JavaScript, React, React-Native, PHP/Laravel, Bootstrap, REST API, CKEditor

Full Stack Developer | Studio 14, Birmingham
December 2019 – August 2022
- Adopted DevOps practices streamlining frontend development; architected and developed a mobile-first frontend system using Web Components and a headless CMS, and mentored the development team in adopting the architecture.
- Accelerated the team's kanban delivery by 28% through translating product requirements into technical ticket scopes, establishing clear timelines, and optimizing Kanban workflows.
- Developed an e-commerce PWA and dashboard for 100+ businesses, optimized Core Web Vitals, and reduced network payload sizes, improving performance scores from 52% to 98%.
- Integrated automated WCAG accessibility and HTML validation into the CI/CD pipeline and hardened application security (XSS/CSRF prevention) across a production-ready application.
Tools: JavaScript/TypeScript, Web Components, ReactJS, Redux, NextJS, GraphQL, SWR, Styled Components, Tailwind CSS, Storybook, SCSS, WordPress CMS, Brotli, AWS

Frontend Engineer | EveryFarmer, Abuja
March 2019 – July 2020
- Built an interactive geospatial mapping tool using Google Maps in collaboration with the product design team and CTO to automate farm mapping assessments for 100,000+ service providers, reducing operating expenses by $10,000.
- Introduced static analysis and CI/CD pipelines with GitHub Actions, streamlining development workflow and improving developer experience through automated quality checks and delivery.
- Strengthened system reliability and maintainability with Redux Saga for state management and implemented comprehensive unit and integration tests (Jest, RTL), resulting in 80% test coverage.
Tools: JavaScript, React, Redux, Redux-saga, Bootstrap, React-testing-library, Jest, Java RESTful API, Google Maps, Cypress

EDUCATION
M.Sc. Strategic Information Systems Management — Stockholm University (2023–2026)
B.Sc. Business Administration & Management — University of Lagos (2012–2015)

CERTIFICATIONS
GitHub Copilot — Microsoft (September 2025)
Fundamental Training in Product Management — Tredford (August 2021)
Google Mobile Web Specialist (April 2018)
```

### Updating the default resume

If the user says "update my resume," "use this resume instead," or uploads a new file, switch to the new version for all subsequent audits in the session. Confirm the switch with a single line: "Resume updated — I'll use this for future audits."

---

## Required inputs

1. **Resume** — use the default above unless the user has provided or updated one this session.
2. **Job posting** — actual text or a fetchable URL. If a URL is given, fetch it before auditing. Never paraphrase or guess at a posting's content.
   If the job posting is missing, ask for it directly. Don't audit without it.

---

## Process

### 1. Parse the job posting

Extract in order of ATS weight:

- **Hard requirements**: tools, languages, frameworks, certifications, years of experience, degree, location/authorization constraints
- **Repeated/emphasized terms**: keywords in the title, in multiple bullets, or in a "must have" / "required" section — these carry the most ATS weight
- **Title and seniority signal**: exact job title, level words (Senior/Staff/Lead/Principal)
- **Nice-to-haves**: "preferred" or "bonus" items — lower weight, still scored
- **Soft/domain signals**: industry, culture phrases ("fast-paced," "cross-functional") — low ATS weight, moderate human-recruiter weight

### 2. Parse the resume

Extract: skills and tools (note where each appears — summary, bullets, or skills section only), job titles, employment dates, years of relevant experience, education, certifications, and exact keyword forms used (e.g. "JS" vs "JavaScript" — ATS matching is often literal).

### 3. Cross-match and score

Score against this rubric — always show the breakdown, never just a single number:

| Category                      | Weight | What it measures                                                            |
| ----------------------------- | ------ | --------------------------------------------------------------------------- |
| Hard skill / keyword coverage | 40%    | Required + emphasized keywords present in resume, in a form ATS would catch |
| Title & seniority alignment   | 15%    | Resume titles/scope match the posting's level                               |
| Experience & domain match     | 15%    | Years, industry, and project types align                                    |
| Education / certifications    | 10%    | Degree and certs meet stated requirements                                   |
| Parsing risk                  | 20%    | Structural/formatting issues causing ATS to misread — acts as a deduction   |

Report the overall score as a **range** (e.g. "72–78%"), not a single number.

### 4. Parsing-risk checklist

Flag any of these — they cause strong content to score far lower than it deserves:

- Multi-column layouts, tables, or text boxes (ATS reads left-to-right and scrambles these)
- Skills/dates in images, graphics, icons, headers, or footers
- Non-standard section headers
- Missing a plain-text skills section
- Unusual date formats or missing dates
- Contact info only in a header/footer
- Heavily designed or image-based PDF

### 5. Output format

Structure every audit as:

1. **Overall match estimate** (range) — one line on what's driving it
2. **Score breakdown table** — the 5 categories with a short note per row
3. **Missing/weak keywords** — ranked by ATS weight, highest first
4. **Parsing risk flags** — if any
5. **Action plan to reach 90–98%**, split into:
   - _Quick wins_ — keyword/wording changes to existing true experience (minutes)
   - _Restructuring_ — reordering, retitling, reformatting for parseability
   - _Real gaps_ — things no rewording fixes; name these plainly

---

## Principles

- **Never invent or inflate.** Only recommend adding a keyword if it maps to something genuinely in the person's background. If a required keyword has no honest backing, name it as a real gap.
- **Keyword optimisation has a ceiling.** Flag the trade-off if a recommendation risks making the resume keyword-stuffed and unreadable to the human reviewer.
- **Be direct about score confidence.** If the posting is vague or the resume is thin in a load-bearing area, say so rather than presenting false precision.
- **90–98% is a target, not a guarantee.** Frame the plan as closing the gap on factors that matter most — not a score the user can bank on.
- **The default resume is a starting point.** If the user's actual experience differs from what's stored, use what they tell you — don't argue from the stored version.
