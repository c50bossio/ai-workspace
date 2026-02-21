# Bossio AI Team Architecture
*Version 1.0 — February 21, 2026*

## 🏢 Organization Chart

```
                    ┌─────────────────────┐
                    │      BOSSIO         │
                    │   (Human CEO)       │
                    └──────────┬──────────┘
                               │
              ┌────────────────┴────────────────┐
              │                                 │
              ▼                                 ▼
┌─────────────────────────┐       ┌─────────────────────────┐
│   ATLAS (MacBook Pro)   │       │   TYLER (Mac Mini)      │
│   COO / Orchestrator    │◄─────►│   CTO / Lead Engineer   │
│   Model: Opus 4.5       │Discord│   Model: Opus 4.5       │
└───────────┬─────────────┘       └───────────┬─────────────┘
            │                                 │
    ┌───────┼───────┬───────┐        ┌───────┼───────┬───────┐
    │       │       │       │        │       │       │       │
    ▼       ▼       ▼       ▼        ▼       ▼       ▼       ▼
┌───────┐┌───────┐┌───────┐┌─────┐┌───────┐┌───────┐┌──────┐┌───────┐
│Research││Content││ Scout ││Image││ Coder ││Debuggr││Tester││Auditor│
│Sonnet ││Sonnet ││Gemini ││Sonnet││Sonnet ││Sonnet ││Sonnet││Sonnet │
└───────┘└───────┘└───────┘└─────┘└───────┘└───────┘└──────┘└───────┘
```

---

## 👤 ATLAS TEAM (MacBook Pro)

### Atlas — COO / Orchestrator
| Attribute | Value |
|-----------|-------|
| **Model** | Opus 4.5 |
| **Machine** | MacBook Pro |
| **Channel** | Telegram (Bossio), Discord (#ai-workspace) |

**Responsibilities:**
- All direct communication with Bossio
- Strategic planning and business decisions
- Coordinating work between teams
- Reviewing deliverables before presenting to Bossio
- Managing cron jobs and automations
- Ad performance strategy and recommendations

**Does NOT do:**
- Write code directly
- Deep research (delegates to Researcher)
- Content creation (delegates to Content)
- Routine monitoring (delegates to Scout)

---

### Researcher — Research Specialist
| Attribute | Value |
|-----------|-------|
| **Model** | Sonnet 4.5 |
| **Type** | Sub-agent (spawned by Atlas) |

**Responsibilities:**
- Web searches for trends, competitors, news
- Market research and analysis
- Reading/summarizing articles, videos, docs
- Gathering social media insights
- Competitor analysis

**Output:** Structured research reports with sources and confidence levels

---

### Content — Content Creator
| Attribute | Value |
|-----------|-------|
| **Model** | Sonnet 4.5 |
| **Type** | Sub-agent (spawned by Atlas) |

**Responsibilities:**
- Social media posts (X, Instagram, TikTok)
- Video scripts and outlines
- Marketing copy (ads, emails, landing pages)
- Blog posts and articles
- Content calendars

**Output:** Ready-to-review drafts matching brand voice

---

### Scout — Monitoring Specialist
| Attribute | Value |
|-----------|-------|
| **Model** | Gemini Flash |
| **Type** | Sub-agent (spawned by Atlas) |

**Responsibilities:**
- Heartbeat monitoring
- Alert queue checks
- Simple deployment verifications
- Basic health checks
- Routine data lookups

**Output:** Quick status reports or HEARTBEAT_OK

---

### Image — Visual Creator
| Attribute | Value |
|-----------|-------|
| **Model** | Sonnet 4.5 + Nano Banana Pro |
| **Type** | Sub-agent (spawned by Atlas) |

**Responsibilities:**
- Generate thumbnails
- Create ad creative images
- Social media graphics
- Product mockups

**Output:** Image files ready for use

---

## 🔧 TYLER TEAM (Mac Mini)

### Tyler — CTO / Lead Engineer
| Attribute | Value |
|-----------|-------|
| **Model** | Opus 4.5 |
| **Machine** | Mac Mini |
| **Channel** | Discord (#ai-workspace) |

**Responsibilities:**
- All engineering decisions and architecture
- Coordinating coding sub-agents
- Code review approval
- Deployment decisions
- Technical problem-solving
- Communicating with Atlas on technical matters

**Does NOT do:**
- Direct communication with Bossio (goes through Atlas)
- Business/strategy decisions
- Marketing/content work

---

### Coder — Implementation Specialist
| Attribute | Value |
|-----------|-------|
| **Model** | Sonnet 4.5 |
| **Type** | Sub-agent (spawned by Tyler) |

**Responsibilities:**
- Write new features
- Implement designs
- Create database migrations
- Build API endpoints
- Frontend components

**Output:** Working code, committed to branch

---

### Debugger — Bug Fix Specialist
| Attribute | Value |
|-----------|-------|
| **Model** | Sonnet 4.5 |
| **Type** | Sub-agent (spawned by Tyler) |

**Responsibilities:**
- Investigate bug reports
- Root cause analysis
- Fix bugs
- Write regression tests
- Document fixes

**Output:** Bug fixes with explanation of root cause

---

### Tester — QA Specialist
| Attribute | Value |
|-----------|-------|
| **Model** | Sonnet 4.5 |
| **Type** | Sub-agent (spawned by Tyler) |

**Responsibilities:**
- Run test suites
- Manual testing verification
- Test edge cases
- Verify deployments work
- Browser/mobile testing

**Output:** Test reports with pass/fail status

---

### Auditor — Code Review Specialist
| Attribute | Value |
|-----------|-------|
| **Model** | Sonnet 4.5 |
| **Type** | Sub-agent (spawned by Tyler) |

**Responsibilities:**
- Review all code before merge
- TypeScript/lint checks
- Security review
- Code quality assessment
- Enforce coding standards
- Check for code smells

**Output:** Review report with approve/request-changes

---

## 🔄 Workflows

### Standard Task Flow
```
1. Bossio → Atlas (request)
2. Atlas analyzes, plans approach
3. Atlas delegates to appropriate agent(s):
   - Coding → Tyler
   - Research → Researcher
   - Content → Content
   - Monitoring → Scout
4. Agent(s) complete work
5. Atlas reviews
6. Atlas reports to Bossio
```

### Coding Task Flow
```
1. Atlas → Tyler (task via Discord)
2. Tyler plans implementation
3. Tyler → Coder (implementation)
4. Coder completes → Tyler
5. Tyler → Auditor (code review)
6. If issues → back to Coder/Debugger
7. If approved → Tyler → Tester
8. Tester verifies → Tyler
9. Tyler approves merge & deploy
10. Tyler → Atlas (completion report)
11. Atlas → Bossio (done)
```

### Content Pipeline Flow (Automated)
```
1. Scout scans X for trending content (cron)
2. Researcher analyzes trends
3. Content creates drafts
4. Image generates visuals
5. Atlas reviews batch
6. Ready for Bossio approval
```

---

## 📅 Cron Jobs

### Atlas Machine (MacBook Pro)
| Job | Schedule | Agent | Task |
|-----|----------|-------|------|
| Heartbeat | Every 30min | Scout | Check alerts, system health |
| Content Scan | 6 AM daily | Researcher | Scan X for trending content |
| Content Draft | 7 AM daily | Content | Generate post drafts |
| Ad Learning | Sunday 5 AM | Atlas | Creative performance analysis |

### Tyler Machine (Mac Mini)
| Job | Schedule | Agent | Task |
|-----|----------|-------|------|
| Build Check | Every push | Auditor | TypeScript/lint verification |
| Deploy Verify | After deploy | Tester | Smoke test production |

---

## 🛠️ Implementation Checklist

### Atlas Side (MacBook Pro)
- [x] Update SOUL.md to Orchestrator role
- [x] Update AGENTS.md with team roster
- [x] Create agent-configs/ directory
- [x] Create RESEARCHER_SOUL.md
- [x] Create CONTENT_SOUL.md
- [x] Create SCOUT_SOUL.md
- [x] Create IMAGE_SOUL.md
- [x] Creative Learning Loop deployed
- [ ] Configure Scout on Gemini Flash model
- [ ] Set up content pipeline cron jobs
- [ ] Test researcher spawning
- [ ] Test content spawning

### Tyler Side (Mac Mini)
- [ ] Update to Opus 4.5 model
- [ ] Update SOUL.md to CTO role
- [ ] Create CODER_SOUL.md
- [ ] Create DEBUGGER_SOUL.md
- [ ] Create TESTER_SOUL.md
- [ ] Create AUDITOR_SOUL.md
- [ ] Set up agent spawning workflow
- [ ] Test full coding pipeline

---

## 💰 Cost Optimization

| Agent | Model | Est. Cost/Day |
|-------|-------|---------------|
| Atlas | Opus | ~$5-10 (strategy only) |
| Tyler | Opus | ~$5-10 (engineering decisions) |
| Researcher | Sonnet | ~$1-2 |
| Content | Sonnet | ~$1-2 |
| Coder | Sonnet | ~$2-5 |
| Debugger | Sonnet | ~$0.50-1 |
| Tester | Sonnet | ~$0.50-1 |
| Auditor | Sonnet | ~$0.50-1 |
| Scout | Gemini Flash | ~$0.10-0.20 |

**Estimated Total:** ~$15-30/day vs. one Opus doing everything (~$50+/day)

---

## 🎯 Success Metrics

1. **Response Time:** Bossio gets answers faster (parallel work)
2. **Code Quality:** Fewer bugs (Auditor catches issues)
3. **Cost:** Lower API spend (right model for right task)
4. **Output:** More content, more features shipped
5. **Uptime:** Automated monitoring catches issues early

---

*Document maintained by Atlas. Last updated: Feb 21, 2026*
