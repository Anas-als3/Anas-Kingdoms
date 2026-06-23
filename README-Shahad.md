# Shahad — Kingdom API

**الممالك (Kingdom)** منصّة عربية لتطوير الذات بأسلوب الألعاب (Spring Boot + MySQL، مصادقة HTTP Basic). ينضمّ اللاعب إلى **ممالك** متنوّعة (اللياقة، الصدقة، التطوّع، القراءة، الألعاب، الإيمان، المعرفة، التغذية، البرمجة)، ويخوض **تحدّيات يولّدها الذكاء الاصطناعي**، وعليه أن **يُثبت** إكمال كل تحدٍّ عبر تحقّق حقيقي — أنشطة Strava، أو تبرّعات بنكية عبر المصرفية المفتوحة (Neotek)، أو شهادات تطوّع يراجعها الذكاء الاصطناعي، أو اختبارات على واتساب، أو تحليل صور الوجبات، أو وقت اللعب والإنجازات على Steam، أو مساهمات GitHub. التحدّيات المُثبَتة تمنح **نقاط خبرة (XP)** ترفع **درجة اللاعب** (من D3 إلى D1) وتُكسبه **أوسمة**؛ كما **يتنافس اللاعبون في لوبيات** (يفتح اشتراك Premium عبر LemonSqueezy إنشاءها)، وتُرسل أتمتة **n8n** تذكيرات عبر واتساب والبريد الإلكتروني.

**Shahad owns** the kingdom discovery + progression surface: AI kingdom recommendation, joining kingdoms, leaderboards & land control, per-kingdom XP/streak/division/rank stats, player profile + AI report, earned badges, the admin badge catalog, and the WhatsApp quiz questions for the Reading/Gaming/Faith kingdoms.

## Get Kingdom Recommendation by AI
AI suggests the best-fit kingdom for the logged-in player based on their answers.
- `POST /api/v1/kingdom/ai-recommendation` — submit the player's preferences and get an AI-recommended kingdom.

## View / Join Kingdoms
Browse kingdoms and join one to start earning XP.
- `POST /api/v1/kingdom-membership/join/{kingdomId}` — join a kingdom (returns an Arabic welcome message).
- `DELETE /api/v1/kingdom-membership/leave/{kingdomId}` — leave a kingdom.
- `GET /api/v1/kingdom-membership/{kingdomId}/membership-id` — resolve the caller's membership id for a kingdom (used by submit/donate flows).

## View Leaderboard
Per-kingdom rankings and "land control" share, sliceable by period and division.
- `GET /api/v1/kingdom/{kingdomId}/leaderboard/period/{period}` — leaderboard for a kingdom over a period.
- `GET /api/v1/kingdom/{kingdomId}/leaderboard/division/{division}` — leaderboard for a kingdom within a division.
- `GET /api/v1/kingdom/{kingdomId}/leaderboard/period/{period}/division/{division}` — leaderboard filtered by both period and division.
- `GET /api/v1/kingdom/{kingdomId}/land-control/{division}` — land-control summary (territory share) for a division.

## Earn XP / Badge (player progression stats)
The logged-in player's standing inside a given kingdom.
- `GET /api/v1/kingdom-membership/{kingdomId}/member-xp` — current XP in the kingdom.
- `GET /api/v1/kingdom-membership/{kingdomId}/member-streak` — current streak.
- `GET /api/v1/kingdom-membership/{kingdomId}/member-divison` — current division.
- `GET /api/v1/kingdom-membership/{kingdomId}/member-rank` — current rank.
- `GET /api/v1/kingdom-membership/{kingdomId}/member-land-percentage` — share of land controlled.
- `GET /api/v1/kingdom-membership/{kingdomId}/xp-need-to-higher-rank` — XP remaining to climb a rank.
- `GET /api/v1/kingdom-membership/{kingdomId}/number-of-completed-challenges` — completed-challenge count.
- `GET /api/v1/kingdom-membership/{kingdomId}/division-progress` — progress toward the next division.
- `GET /api/v1/player-badge/player-badges` — all badges the player has earned.
- `GET /api/v1/player-badge/{kingdomId}/member-badges` — badges earned within one kingdom.

## Generate AI Player Report
Cross-kingdom profile reads plus an AI-written performance report.
- `GET /api/v1/player/me` — the authenticated caller's identity/profile.
- `GET /api/v1/player/summary` — overall player summary across kingdoms.
- `GET /api/v1/player/kingdoms` — kingdoms the player belongs to.
- `GET /api/v1/player/best-kingdom` — the player's strongest kingdom.
- `GET /api/v1/player/highest-streak` — the player's best streak.
- `POST /api/v1/player/ai-report` — generate an AI player report and email it.

## Manage Badges (admin)
Admin maintains the badge catalog that progression awards draw from.
- `POST /api/v1/badge/add/{kingdomId}` — 🔒 admin · create a badge for a kingdom.
- `PUT /api/v1/badge/update/{id}` — 🔒 admin · update a badge.
- `DELETE /api/v1/badge/delete/{id}` — 🔒 admin · delete a badge.

## Quiz Questions (Reading / Gaming / Faith)
The WhatsApp quiz channel that verifies his kingdoms' challenges.
- `POST /api/v1/challenge-question/whatsapp/webhook` — receive a player's WhatsApp quiz reply and return the next prompt / result.

## Data
Shahad mainly works with **Kingdom, KingdomMembership, PeriodScore, Player, Badge, PlayerBadge, ChallengeQuestion** (and reads ChallengeProgress for completion counts).

> Diagrams: see the project use-case diagram and class diagram.
