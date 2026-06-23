# Anas — Kingdom API

**الممالك (Kingdom)** منصّة عربية لتطوير الذات بأسلوب الألعاب (Spring Boot + MySQL، مصادقة HTTP Basic). ينضمّ اللاعب إلى **ممالك** متنوّعة (اللياقة، الصدقة، التطوّع، القراءة، الألعاب، الإيمان، المعرفة، التغذية، البرمجة)، ويخوض **تحدّيات يولّدها الذكاء الاصطناعي**، وعليه أن **يُثبت** إكمال كل تحدٍّ عبر تحقّق حقيقي — أنشطة Strava، أو تبرّعات بنكية عبر المصرفية المفتوحة (Neotek)، أو شهادات تطوّع يراجعها الذكاء الاصطناعي، أو اختبارات على واتساب، أو تحليل صور الوجبات، أو وقت اللعب والإنجازات على Steam، أو مساهمات GitHub. التحدّيات المُثبَتة تمنح **نقاط خبرة (XP)** ترفع **درجة اللاعب** (من D3 إلى D1) وتُكسبه **أوسمة**؛ كما **يتنافس اللاعبون في لوبيات** (يفتح اشتراك Premium عبر LemonSqueezy إنشاءها)، وتُرسل أتمتة **n8n** تذكيرات عبر واتساب والبريد الإلكتروني.

Anas owns registration/OTP auth, browsing & joining AI-generated challenges, the full challenge-completion + verification stack (Strava fitness, Neotek charity, AI volunteer-PDF, plus the WhatsApp quiz/nutrition inbound it routes), external-account linking, and the lobby resolve — for the **Fitness, Charity, and Volunteer** kingdoms.

## Register & Login
Create an account and confirm the player's phone via a WhatsApp OTP (identity comes from the HTTP Basic login, never a phone param).
- `POST /api/v1/auth/register` — create User + Player + memberships and send the verification OTP over WhatsApp (public).
- `POST /api/v1/auth/send-otp` — resend the verification code to the logged-in user's phone.
- `POST /api/v1/auth/verify-otp` — verify the code (`?code=`); on success sends the welcome email and activates the account.

## View/Join Challenges
Browse AI-generated challenges by kingdom/difficulty/period and join one to start a run.
- `GET /api/v1/challenge/kingdom/{kingdomId}` — list challenges for a kingdom (browse-by-kingdom).
- `GET /api/v1/challenge/difficulty/{difficulty}` — list challenges filtered by difficulty.
- `GET /api/v1/challenge/period/{period}` — list challenges filtered by period (daily/weekly/etc.).
- `POST /api/v1/challenge-progress/join/{challengeId}` — the logged-in player joins a challenge, creating a progress run.

## Complete Challenge
Drive a joined challenge run to completion, cancel it, or read the player's own runs.
- `POST /api/v1/challenge-progress/finish/{id}` — finish/complete a challenge run.
- `POST /api/v1/challenge-progress/cancel/{id}` — cancel an in-progress run.
- `POST /api/v1/challenge-progress/submit-image/{progressId}` — submit a nutrition food-photo (`image` multipart) for AI analysis.
- `GET /api/v1/challenge-progress/player` — the logged-in player's challenge runs.
- `GET /api/v1/challenge-progress/player/active` — the player's currently active runs.
- `GET /api/v1/challenge-progress/player/status/{status}` — the player's runs filtered by status.

## Verify Challenge Completion
Prove a challenge through real external verification — Strava fitness, Neotek Open-Banking donations, and AI-reviewed volunteer PDFs; on approval the run is completed and XP awarded.
- `GET /api/v1/verify/fitness/connect` — get the player's Strava authorize URL to link their account.
- `GET /api/v1/verify/fitness/callback` — Strava OAuth redirect target; stores the player's refresh token.
- `GET /api/v1/verify/fitness/activities` — list the player's recent Strava activities (lean view).
- `GET /api/v1/verify/fitness/check` — check whether a fitness goal (distance/time/count) was reached.
- `POST /api/v1/verify/charity/link` — start Neotek bank consent; returns the bank authorize URL + accountsLinkId.
- `GET /api/v1/verify/charity/accounts` — list the player's linked bank accounts.
- `GET /api/v1/verify/charity/transactions` — list the player's bank transactions.
- `GET /api/v1/verify/charity/check` — check whether a qualifying donation was made.
- `POST /api/v1/verify/charity/donate` — demo helper: simulate a donation so a charity check can pass.
- `POST /api/v1/verify/charity/manual-donate/{kingdomMembershipId}/{challengeId}` — record a member's donation (SAR) so a charity lobby can rank by total donated.
- `POST /api/v1/verify/volunteer/upload` — upload a certificate PDF (`file`); AI verifies it and, with `progressId`, completes the run + awards XP.

## Link External Account
Link/unlink the third-party accounts (Strava, bank, etc.) that verification reads from.
- `POST /api/v1/connecte/link` — link an external provider account to the logged-in player.
- `GET /api/v1/connecte/player` — list the player's linked accounts (token-free).
- `DELETE /api/v1/connecte/disconnect/{provider}` — unlink a provider for the logged-in player.

## Generate Challenge by AI
Admin-triggered AI generation of a new challenge for a kingdom.
- `POST /api/v1/challenge/generate` — AI generates + saves a challenge for `?kingdomId&difficulty&period`. 🔒 admin

## WhatsApp Inbound
The single inbound webhook that routes WhatsApp messages by content — volunteer PDFs, food images, quiz answers, and lobby-invite accept/decline taps.
- `POST /api/v1/verify/volunteer/whatsapp` — Twilio webhook: routes PDF→volunteer verify, image→nutrition, قبول/رفض→lobby invite, else→quiz answer (TwiML reply, optional signature check).
- `POST /api/v1/whatsapp/webhook` — inbound nutrition food-image webhook (async meal analysis, TwiML ack).

## Streak Keeper
On-demand triggers for the per-kingdom daily streak (real schedule runs via cron at 18:00 warn / 00:05 reset).
- `POST /api/v1/verify/streak/run` — run the streak reset/warn pass now (demo).
- `POST /api/v1/verify/streak/warn/{playerId}/{kingdomId}` — send the "streak ends in 6 hours" WhatsApp warning for one player+kingdom.

## Resolve Lobby
Close out a lobby competition and award the winner.
- `POST /api/v1/lobby/finish/{lobbyId}/{winnerPlayerId}` — finish the lobby and record the winning player.

## Data
Anas mainly works with **User**, **Player**, **ConnectedAccount**, **Challenge**, **ChallengeProgress**, **KingdomMembership**, and **Lobby** (resolve only) — across the Fitness, Charity, and Volunteer kingdoms.

> Diagrams: see the project use-case diagram and class diagram.
