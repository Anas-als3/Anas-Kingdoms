# Maysun — Kingdom API

**الممالك (Kingdom)** منصّة عربية لتطوير الذات بأسلوب الألعاب (Spring Boot + MySQL، مصادقة HTTP Basic). ينضمّ اللاعب إلى **ممالك** متنوّعة (اللياقة، الصدقة، التطوّع، القراءة، الألعاب، الإيمان، المعرفة، التغذية، البرمجة)، ويخوض **تحدّيات يولّدها الذكاء الاصطناعي**، وعليه أن **يُثبت** إكمال كل تحدٍّ عبر تحقّق حقيقي — أنشطة Strava، أو تبرّعات بنكية عبر المصرفية المفتوحة (Neotek)، أو شهادات تطوّع يراجعها الذكاء الاصطناعي، أو اختبارات على واتساب، أو تحليل صور الوجبات، أو وقت اللعب والإنجازات على Steam، أو مساهمات GitHub. التحدّيات المُثبَتة تمنح **نقاط خبرة (XP)** ترفع **درجة اللاعب** (من D3 إلى D1) وتُكسبه **أوسمة**؛ كما **يتنافس اللاعبون في لوبيات** (يفتح اشتراك Premium عبر LemonSqueezy إنشاءها)، وتُرسل أتمتة **n8n** تذكيرات عبر واتساب والبريد الإلكتروني.

**Maysun owns** the full lobby/competition stack (lobbies, members, invites, lobby-challenges), subscriptions & LemonSqueezy payments, n8n reminder automations, and the entire admin panel.

## View / Join Lobbies
Browse public/available lobbies by kingdom, join open lobbies or a private one by invite code, and see your own lobbies.
- `GET /api/v1/lobby/public/{kingdomId}` — list public lobbies in a kingdom (joinable by the player)
- `GET /api/v1/lobby/available` — list lobbies the player can currently join
- `GET /api/v1/lobby/my` — lobbies the player belongs to
- `GET /api/v1/lobby/active` — the player's currently-active lobbies
- `GET /api/v1/lobby/my-private/{kingdomId}` — the player's private lobbies in a kingdom
- `GET /api/v1/lobby/host/{lobbyId}` — who hosts a lobby
- `GET /api/v1/lobby/{lobbyId}/member-count` — number of members in a lobby
- `GET /api/v1/lobby-member/members/{lobbyId}` — list members of a lobby
- `POST /api/v1/lobby-member/join/{lobbyId}` — join an open lobby
- `POST /api/v1/lobby-member/join-private/{inviteCode}` — join a private lobby via its invite code
- `DELETE /api/v1/lobby-member/leave/{lobbyId}` — leave a lobby

## Get Lobby Recommendation by AI
Suggest the best lobby for the player based on their kingdoms/activity.
- `GET /api/v1/lobby/suggest` — AI-recommended lobby for the player

## Create Lobby (Public / Private → Invite Player)
Premium players create a public or private lobby on a kingdom challenge, then manage and invite players into it.
- `POST /api/v1/lobby/create/{kingdomId}/{challengeId}` — create a lobby (public or private) on a challenge; returns `lobbyId`
- `PUT /api/v1/lobby/update/{lobbyId}` — edit lobby settings (host)
- `DELETE /api/v1/lobby/cancel/{lobbyId}` — host cancels their lobby
- `DELETE /api/v1/lobby-member/kick/{lobbyId}/{targetPlayerId}` — host kicks a member
- `PUT /api/v1/lobby-member/update-role/{memberId}/{role}` — change a member's role
- `POST /api/v1/invite/send/{lobbyId}/{username}` — invite a player to the lobby by username
- `POST /api/v1/invite/resend/{inviteId}` — resend a pending invite
- `GET /api/v1/invite/my-pending` — the player's pending lobby invites
- `PUT /api/v1/invite/update-status/{inviteId}/{status}` — accept/respond to an invite
- `PUT /api/v1/invite/reject/{inviteId}` — reject a received invite

## Compete in Lobby Challenge
Lobby members race the same challenge; standings and results surface here.
- `GET /api/v1/lobby/winners` — recent lobby winners (also feeds n8n)
- `GET /api/v1/lobby/my-finished` — the player's finished lobbies

## Manage Subscription
Read premium status / countdown, manage the player's own subscription, and trigger expiry reminders.
- `GET /api/v1/subscription/getDetails` — the player's subscription details
- `GET /api/v1/subscription/am-i-premium` — whether the authenticated player is premium
- `GET /api/v1/subscription/days-left` — days remaining on the subscription
- `POST /api/v1/subscription/cancel` — cancel the player's own subscription
- `POST /api/v1/subscription/notify-expiry` — push the player's expiry reminder through n8n (WhatsApp/email)
- `GET /api/v1/subscription/premium-active` — list all active premium subscribers 🔒 admin
- `GET /api/v1/subscription/subscriptions-expiring` — soon-to-expire subscriptions (n8n feed) 🔒 admin

## Premium Payment (LemonSqueezy)
Premium checkout/renewal via LemonSqueezy, plus the payment webhook that activates premium.
- `GET /api/v1/payment/plans` — available premium plans
- `POST /api/v1/payment/checkout/{plan}` — create a LemonSqueezy checkout for a plan
- `POST /api/v1/payment/renew/{plan}` — renew premium on a plan
- `POST /api/v1/payment/webhook` — LemonSqueezy webhook → activates/updates the subscription

## n8n Reminder Automations
Trigger and feed the n8n workflows that relay WhatsApp/email reminders.
- `POST /api/v1/n8n/test` — fire a sample payload at the configured n8n webhook to verify the pipe 🔒 admin
- `GET /api/v1/lobby/winners` — winners feed consumed by n8n
- `GET /api/v1/subscription/subscriptions-expiring` — expiry feed consumed by n8n 🔒 admin

## Admin Panel
Full admin oversight of users, players, challenges, lobbies, subscriptions, and challenge-progress review. 🔒 admin
- `GET /api/v1/user/statistics` — platform stats dashboard 🔒 admin
- `GET /api/v1/user/players` — list all players 🔒 admin
- `GET /api/v1/user/challenges` — list all challenges 🔒 admin
- `GET /api/v1/user/subscriptions` — list all subscriptions 🔒 admin
- `GET /api/v1/user/lobbies` — list all lobbies 🔒 admin
- `GET /api/v1/user/kingdoms` — list all kingdoms 🔒 admin
- `GET /api/v1/user/kingdom/{kingdomId}` — kingdom detail 🔒 admin
- `GET /api/v1/user/challenge-progress` — all challenge-progress records 🔒 admin
- `GET /api/v1/user/challenge-progress/pending` — submissions pending review 🔒 admin
- `PUT /api/v1/user/player/{playerId}/ban` — ban a player 🔒 admin
- `PUT /api/v1/user/player/{playerId}/unban` — unban a player 🔒 admin
- `PUT /api/v1/user/membership/{membershipId}/adjust-xp/{xp}` — manually adjust a player's XP 🔒 admin
- `DELETE /api/v1/user/lobby/{lobbyId}/force-delete` — force-delete any lobby 🔒 admin

## Data
Maysun mainly works with **Lobby, LobbyMember, LobbyInvite, Subscription, Player, User** (and reads Kingdom, Challenge, ChallengeProgress for the admin panel).

> Diagrams: see the project use-case diagram and class diagram.
