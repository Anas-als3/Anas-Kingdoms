# الممالك — Kingdoms

**الممالك (Kingdom)** منصّة عربية لتطوير الذات بأسلوب الألعاب (Spring Boot + MySQL، مصادقة HTTP Basic). ينضمّ اللاعب إلى **ممالك** متنوّعة (اللياقة، الصدقة، التطوّع، القراءة، الألعاب، الإيمان، المعرفة، التغذية، البرمجة)، ويخوض **تحدّيات يولّدها الذكاء الاصطناعي**، وعليه أن **يُثبت** إكمال كل تحدٍّ عبر تحقّق حقيقي — أنشطة Strava، أو تبرّعات بنكية عبر المصرفية المفتوحة (Neotek)، أو شهادات تطوّع يراجعها الذكاء الاصطناعي، أو اختبارات على واتساب، أو تحليل صور الوجبات، أو وقت اللعب والإنجازات على Steam، أو مساهمات GitHub. التحدّيات المُثبَتة تمنح **نقاط خبرة (XP)** ترفع **درجة اللاعب** (من D3 إلى D1) وتُكسبه **أوسمة**؛ كما **يتنافس اللاعبون في لوبيات** (يفتح اشتراك Premium عبر LemonSqueezy إنشاءها)، وتُرسل أتمتة **n8n** تذكيرات عبر واتساب والبريد الإلكتروني.

> Kingdom is an Arabic-first gamified self-improvement platform: players join themed kingdoms, take AI-generated challenges, prove them through real verification, earn XP/badges, climb divisions, and compete in lobbies.

## Tech stack
Spring Boot (Java 17) · MySQL · HTTP Basic auth. Integrations: OpenAI, Twilio (WhatsApp), Mailtrap (email), Strava, Steam, Neotek Open Banking, Google Books, LemonSqueezy, n8n.

## The 9 kingdoms (each verified differently)
| Kingdom | Verified by |
|---|---|
| Fitness | Strava activities |
| Charity | bank donations via Open Banking (Neotek) |
| Volunteer | AI-reviewed certificate PDFs |
| Reading / Faith / Knowledge | WhatsApp / AI quizzes |
| Gaming | Steam playtime & achievements |
| Nutrition | AI food-photo analysis |
| Programming | GitHub activity |

## Team & API ownership
- **[Anas](README-Anas.md)** — challenges, verification, register, lobby resolve · *Fitness, Charity, Volunteer*
- **[Shahad](README-Shahad.md)** — kingdoms (join + AI suggestions), profile, badges, leaderboards · *Reading, Gaming, Faith*
- **[Maysun](README-Maysun.md)** — lobbies, premium & payments, n8n automations, admin · *Knowledge, Nutrition, Programming*

## Diagrams
- [Use-case diagram](docs/use-case-diagram.svg)
- [Class diagram](docs/class-diagram.svg)

## Run locally
1. Start MySQL and create a database named `Data`.
2. Provide secrets as environment variables (or a git-ignored `src/main/resources/application-local.properties`): `OPENAI_API_KEY`, `TWILIO_ACCOUNT_SID`/`TWILIO_AUTH_TOKEN`, `MAILTRAP_API_TOKEN`, `STRAVA_*`, `SteamKey`, `GOOGLE_BOOKS_API_KEY`, `LEMON_KEY`, `NEOTEK_*`, `GITHUB_API_TOKEN`.
3. `mvn spring-boot:run` → the API is at `http://localhost:8080/api/v1`.

> For deployment, set `SPRING_JPA_HIBERNATE_DDL_AUTO=update` and `DEMO_SEED_ENABLED=false`.
