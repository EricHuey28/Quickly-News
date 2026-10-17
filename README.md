Try it out:
https://mr-news-ashy.vercel.app/


The AI Newsletter That Reads Itself

Six hundred sources. Seven stories. Five minutes. MR NEWS is an autonomous intelligence pipeline that reads the world news so you do not have to, delivering a high signal briefing straight to subscriber inboxes every evening at 22:00 IST.

=
The Problem
Staying informed in technology and artificial intelligence has become exhausting. Between clickbait headlines, repetitive syndications across rival publishers, and breathless social media commentary, readers waste hours sorting signal from noise. Most newsletters are either manually compiled and slow, or simple RSS dumps that are noisy and redundant.

The Solution
MR NEWS is a fully autonomous editorial desk that does the heavy reading for you:

Aggregates top global RSS feeds across technology, AI labs, policy, chips, science, and funding.
Deduplicates cross source stories using SHA256 content hashing and Jaccard similarity to ensure zero repetitive coverage.
Synthesizes the day's top seven most impactful stories using the Google Gemini 1.5 Flash model into clean factual executive takeaways with strategic impact scores.
Delivers an editorial grade briefing directly to each subscriber's inbox every night at exactly 22:00 IST.
Architecture and Pipeline Flow
[ Top RSS Feeds ] (Reuters, AP, BBC, WSJ, TechCrunch, Ars Technica, MIT Tech Review)
         |
         v
[ Ingestion and Normalization ]
         |
         v
[ SHA256 and Jaccard Deduplication Engine ] => Filters repeated stories and noise
         |
         v
[ Google Gemini Chief AI Editor ] => Selects top 7 stories, writes summaries and impact analysis
         |
         v
[ Supabase PostgreSQL ] => Stores curated editions and subscriber preferences
         |
         v
[ Automated Vercel Cron ] (Daily 22:00 IST)
         |
         v
[ Resend Email Delivery ] => Sends personalized responsive HTML briefing to all active subscribers
Key Features
Mathematical Deduplication: Candidate articles are filtered with SHA256 URL hashing and word level Jaccard similarity before reaching the AI model. Stories covering the same event from different outlets are automatically collapsed into one canonical story.
Gemini Chief AI Editor: Google Gemini 1.5 Flash is prompted with strict schema constraints. Every story gets a two sentence factual summary, one sentence of strategic industry impact, a category tag from seven verticals, and an impact score between 60 and 99.
Guaranteed Nightly Delivery: Vercel Cron triggers the pipeline endpoint every evening at 22:00 IST without any manual intervention.
Enterprise Grade Security: Admin access uses timing safe HMAC comparisons. Unsubscribe links use cryptographic SHA256 tokens. Subscription endpoints are rate limited and protected against bot spam.
Real Time Admin Control Desk: A protected dashboard shows active subscriber counts, pipeline health, database latency, and historical briefing archives.
Privacy First: No tracking cookies, no advertising, no data sold. One click unsubscribe is always available via cryptographically signed tokens embedded in every email.
Technology Stack
Layer	Technology
Framework	Next.js 16 App Router, React 18, TypeScript, Vanilla CSS
Backend and APIs	Next.js Serverless Routes, Vercel Cron Jobs
Database	Supabase PostgreSQL with Row Level Security
AI	Google Gemini 1.5 Flash via the Generative AI SDK
Email Delivery	Resend API with custom responsive HTML templates
Deployment	Vercel Serverless Platform
API Reference
Endpoint	Method	Description	Auth
/	GET	Landing page and subscriber onboarding	None
/login	GET	Subscriber access portal	None
/admin	GET	Real time Admin Control Desk	Passkey
/api/subscribe	POST	Create new subscriber and send welcome email	Rate limited
/api/send-briefing	POST	Trigger full pipeline ingestion, synthesis, and dispatch	Bearer CRON_SECRET
/api/cron	GET	Automated cron trigger from Vercel	Vercel Cron
/api/status	GET	System and database health probe	Bearer CRON_SECRET
/api/unsubscribe	GET	One click instant unsubscribe	Cryptographic token
Repository Structure
app: Next.js App Router pages, layouts, and API routes
app/api/cron: Nightly automated briefing generation and dispatch endpoint
app/api/subscribe: Public subscriber intake endpoint
app/api/unsubscribe: HMAC verified one click unsubscription handler
app/admin: Protected admin dashboard interface
app/components: UI components including the signature analog delivery clock
lib/gemini.ts: Gemini Chief AI Editor synthesis and structured JSON output
lib/dedup.ts: SHA256 hashing and Jaccard similarity deduplication algorithms
lib/rss.ts: Feed crawling and article normalization across global newsrooms
lib/email.ts: Responsive HTML newsletter generation and Resend dispatch
lib/supabase.ts: Database client and administrative query helpers
lib/crypto.ts: Cryptographic token generation and HMAC verification
supabase/schema.sql: PostgreSQL schema for subscribers, seen articles, and briefings
Environment Variables
Create a file named .env.local in the root directory using .env.example as the template:

NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key
RESEND_API_KEY=your_resend_api_key
RESEND_FROM_EMAIL=your_verified_sender_email
GEMINI_API_KEY=your_gemini_api_key
CRON_SECRET=your_secret_cron_passcode
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=your_random_secret_string
Getting Started
Clone the repository:
git clone https://github.com/XY-COMBINATOR/mr-news.git
cd mr-news
Install dependencies:
npm install
Set up the database: Open the SQL Editor in your Supabase project dashboard and execute the queries in supabase/schema.sql to initialize the subscribers, articles seen, and briefings tables.

Configure environment variables: Copy .env.example to .env.local and fill in your keys for Supabase, Resend, and Google Gemini.

Start the development server:

npm run dev
Open http://localhost:3000 in your browser.

Production Deployment
The project is configured for zero touch deployment on Vercel:

Import the repository into your Vercel dashboard.
Add all environment variables from .env.local into Vercel project settings.
The nightly cron schedule in vercel.json automatically fires the pipeline endpoint every evening at 22:00 IST.
AI Usage Disclosure
AI coding assistants (Google Antigravity and Gemini) were used as pair programming tools for brainstorming architecture, drafting boilerplate type definitions, debugging Turbopack build issues, and optimizing HTML email layouts. The overall product vision, system architecture, prompt engineering, database schema design, and security defenses were fully designed, verified, and owned by the team.

Open Source Credits
Framework and Runtime: Next.js by Vercel, React, TypeScript
Data and Ingestion: rss-parser for feed consumption, Supabase for PostgreSQL
AI Models: Google Generative AI SDK for Gemini
Email Delivery: Resend for transactional email
What We Learned
Math before AI: Running Jaccard similarity and SHA256 hashing before calling the LLM drastically cut token usage and kept the briefing sharp. Simple algorithms do heavy lifting that language models should not be wasted on.
Prompt constraints over prompting creativity: Telling Gemini what not to say mattered far more than giving it creative latitude. Every buzzword we banned from the output made the briefing noticeably sharper.
Email deliverability is an art form: Getting past spam filters required multipart MIME, RFC 8058 list unsubscribe headers, cryptographic tokens, and pure inline CSS. Modern web CSS is practically useless inside email clients.

License
MIT License. Open for community contributions.
