---
title: "Event 1"
date: 2026-05-30
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Event CD AWS

### Event Information

- **Event Name:** AWS Community Day Event
- **Date & Time:** 09:00, May 30, 2026
- **Location:** 26th Floor, Bitexco Tower, 02 Hai Trieu Street, Saigon Ward, Ho Chi Minh City
- **Role:** Attendee

### Event Objectives
* **Networking and Connection:** Create opportunities for networking and connection among AWS developers, students, and cloud computing professionals in Vietnam.
* **Sharing Practical Knowledge:** Gain hands-on experience and insights through 5 specialized sessions: from learning paths for AWS skills (Cloud Quest, Floci), hackathon experiences, ways to build self-confidence, real-world AI startup development (Tu Vi Dai Viet), to DevOps culture in project operations.
* **Exploring Emerging Trends:** Learn about the latest technology trends such as Generative AI (Amazon Bedrock), Serverless architectures, and local testing optimization techniques.

### Speakers & Topics

1. **Huynh Thai Linh** — Topic: *Level up your AWS skills with Cloud Quest and Floci*
2. **Huynh An Khương, Mai Quoc Anh, Nguyen Tran Minh Quan** — Topic: *Hackathon*
3. **Nguyen Thi Quynh Nhu** — Topic: *Why We Always Need Confidence*
4. **Nghia Tran** — Topic: *Building "Tu Vi Dai Viet" Website by Yourself* (https://tuvidaiviet.com/)
5. **Tran Minh Quan** — Topic: *The Hidden Iceberg of a Project: DevOps Before Disaster*

---

## Detailed Session Content

### Part 1: Level Up Your AWS Skills with Cloud Quest and Floci (Speaker: Huynh Thai Linh)

#### 1. Common Challenges Faced by AWS Beginners
Beginners stepping into AWS often struggle with major psychological and technical barriers:
* **Cost Anxiety:** Constantly checking the billing dashboard for fear of unexpected charges.
* **Forgotten Resources:** Forgetting to terminate or delete resources (e.g., EC2, RDS, NAT Gateways) after testing, resulting in unexpected monthly bills.
* **Free Tier Limits:** Fearing charges despite being within the Free Tier limits.

{{% notice info "Key Message" %}}
**Key Message:** The fear of unexpected costs is the biggest psychological barrier preventing beginners from hands-on learning and practicing on AWS.
{{% /notice %}}

#### 2. AWS Cloud Quest – Free Hands-on Learning Environment
AWS Cloud Quest is a 3D gamified learning platform that provides an engaging, risk-free way for beginners to get started with AWS.
* **Key Features:**
  * **Guided AWS Practice:** Detailed, step-by-step hands-on tutorials.
  * **Beginner-Friendly:** An interactive, intuitive interface tailored for novices.
  * **Mission-Based Learning:** Complete quests to solve real-world problems for a virtual city.
  * **Realistic Emulation:** Practice directly on real AWS resources provided by the platform at no personal cost.
* **Role-Based Digital Badges:** Learners can earn certifications for roles such as:
  * *Cloud Practitioner*
  * *Solutions Architect*
  * *Serverless Developer*
  * *Machine Learning Engineer*
  * *Security Specialist*
* **Benefits:** Offers a structured, step-by-step roadmap, increases motivation through gamification, and removes the complexity of manual environment setup.

#### 3. Floci – Local AWS Testing
Floci is a lightweight, open-source local AWS emulator running on developer machines.

*(Insert Image 4 here)*

* **Key Objectives:**
  * Emulate AWS services locally.
  * Eliminate cloud costs during early development.
  * Enable rapid architecture testing.
* **Key Benefits:**
  * Test AWS-integrated applications without deploying them to a real AWS environment.
  * Reduce cloud spending and speed up the development lifecycle (fast feedback loop).
  * Simplify and accelerate debugging.

{{% notice tip "Key Message" %}}
**Key Message:** Always perform local testing before deploying your architectures to the real AWS Cloud.
{{% /notice %}}

#### 4. Comparing Floci and LocalStack
The session presented an objective comparison between Floci and LocalStack across core criteria:
* **Performance:** Floci boasts up to **138 times** faster startup and response speeds compared to LocalStack.
* **Resource Usage:** Consumes about **11 times** less memory and disk space, thanks to optimization with GraalVM Native Image.
* **Service Coverage:** LocalStack offers broader service support, while Floci focuses on optimizing core AWS services (S3, SQS, SNS, etc.).
* **Licensing & Cost:** Differences in licensing models and pricing (Floci is fully open-source and free).

#### 5. Limitations of Floci
Despite its powerful features, Floci still has certain limitations when dealing with complex enterprise architectures.

*(Insert Image 6 here)*

* **Example Architecture:** Complex Big Data and Analytics pipelines using services like *MSK (Kafka), Lambda, Kinesis, S3, Glue, Redshift, and QuickSight*.
* **Implications:** Floci cannot emulate 100% of AWS services. For advanced data pipelines, a real AWS environment (or an isolated Staging environment) is still necessary.

{{% notice warning "Conclusion" %}}
**Conclusion:** Floci is highly suitable for learning, rapid prototyping, and local testing, but it cannot fully replace a production-ready AWS environment.
{{% /notice %}}

#### 6. Effective Practical Learning Roadmap
The speaker proposed an optimized 3-stage roadmap for learning AWS:
* **Phase 1: Mind & Architecture**
  * *Tools:* AWS Cloud Quest.
  * *Objectives:* Build a cloud mindset, understand basic AWS architecture, and get familiar with core services.
* **Phase 2: Code & Fast Testing**
  * *Tools:* Floci (local emulation).
  * *Objectives:* Write code, perform rapid integration tests, and practice system design at zero cost.
* **Phase 3: Real Deployment & Production**
  * *Tools:* Real AWS Cloud.
  * *Objectives:* Deploy applications in real-world environments, optimize costs, manage security, and monitor production performance.

---

### Part 2: Hackathon – Learn Fast, Build Real, Grow Under Pressure (Speakers: Huynh An Khương, Mai Quoc Anh, Nguyen Tran Minh Quan - The Ballers Team)

#### 1. What is a Hackathon?
At the beginning of the presentation, the speakers humorously but realistically defined a Hackathon as:
> *"HA! a tons of fun, bug fixes, back pain and sleep deprivation"*

In essence, a Hackathon is an event where teams develop a product in a very short, limited timeframe (typically 24 to 48 continuous hours). During the event, teams must:
* **Solve Real-World Problems:** Tackle difficult challenges derived from real situations.
* **Build Prototypes/MVPs:** Complete a functional test version in the shortest time possible.
* **Work at High Speed:** Demands rapid teamwork and quick decision-making.
* **Experiment Relentlessly:** Continuously iterate on ideas based on feedback from judges or mentors.

* **Core Objective:** Rather than creating a 100% flawless product, the goal is to turn an idea into a working solution as quickly as possible to prove viability and learn through hands-on practice.

#### 2. Why Participate in a Hackathon?
The speakers raised the question: *"Why should you join a Hackathon?"* and answered:

{{% notice info "Message from the Team" %}}
Hackathons offer invaluable hands-on experiences that traditional theoretical learning or standard school projects can rarely match.
{{% /notice %}}

Real values gained include:
* Learning to work effectively under intense, real-time pressure.
* Developing teamwork and problem-solving skills.
* Gaining exposure to and learning from experienced industry mentors and corporate representatives.
* Building a personal portfolio with real, working products.
* Deeply understanding the product development lifecycle from initial concept to MVP.

#### 3. The Team's Hackathon Journey
The emotional 3-day journey of *The Ballers Team* is summarized below:
* **Day 1 – Kickoff:**
  * Began with high spirits and free food (Donuts and Pizza) provided by the organizers.
  * Attended orientation workshops and the opening ceremony.
  * Brainstormed to find and refine their product concept.
  * Saved a member's laptop from RAM overload and started the sleep-deprived run.
* **Day 2 – Development:**
  * The team had intense debates choosing between different directions, namely *SynthHunter* and *The Great Divide*.
  * Ultimately, they chose the most feasible solution to develop within the competition's tight timeline.

#### 4. SynthHunter – AI Voice Verification System
* **The Problem:** The rise of Generative AI has made voice spoofing (Deepfake Voice) incredibly easy, posing severe security risks such as identity fraud and biometric authentication bypasses.
* **The Solution:** The team built **SynthHunter** — a voice verification system that analyzes audio files and classifies them into three states:
  1. *Human Voice (Human)*
  2. *AI Generated Voice (AI Generated)*
  3. *Needs Review (Needs Review)*
* **Mechanism:** Measures advanced acoustic features such as Speech Dynamics, Encoder Behavior, and Temporal Rhythm Analysis to detect signs of AI synthesis.

#### 5. System Architecture
To achieve rapid deployment during the short duration of the hackathon, the team designed a flexible **Serverless** architecture on AWS:
* **AWS API Gateway & AWS Lambda:** Handle asynchronous API requests and business logic execution.
* **Amazon S3:** Store incoming audio files.
* **Amazon Bedrock:** Connect to and utilize Large Language Models (LLMs).
* **Amazon DynamoDB:** A NoSQL database to store transactions and analysis results.
* **Workflow:** User uploads audio file → Stored in S3 → AI model processes it → Analyzed and evaluated → Verification results returned to the user.

#### 6. Challenges Faced
The competition was not without its hurdles:
* **Data Quality:** The audio data provided by the sponsor contained noise and was not purely human voice, complicating model training.
* **Documentation Issues:** Late changes to API documents and website structure forced the team to rewrite documentation at the last minute.
* **Performance:** The system's processing time was relatively high, making it hard to optimize within the 24-48 hour window.

> [!NOTE]
> **Key Lesson:** In a Hackathon, the greatest challenge is often not coding itself, but the ability to adapt and solve unexpected issues on the fly.

#### 7. The Vortox Project
Besides SynthHunter, the team also presented a secondary project named **Vortox**.
* **The Problem:** The biggest barrier for students seeking jobs is often not a lack of technical skills, but a lack of understanding of the entire recruitment process.
* **The Solution:** Vortox supports the applicant journey through an integrated pipeline: CV Screening → Behavioral Evaluation → Technical Evaluation → Application Tracking in a unified system.

#### 8. Day 3 – Pitching
The final sprint:
* Completed a working demo of the product.
* Prepared concise, professional presentation slides.
* Delivered a live demo to the judging panel.
* **Importance:** A good idea needs to be communicated clearly and persuasively to win over the judges.

#### 9. Key Learnings & Lessons Learnt
The Ballers summarized 4 key takeaways:
1. **Start with a real problem:** Always start from the users' pain points rather than the technology. A product that solves a real problem holds far more value than one that only showcases a technology.
2. **Experiment relentlessly:** Be ready to test, adapt, and pivot when the initial approach fails. Many small iterations are better than trying to execute a perfect plan on paper.
3. **Persistence matters:** Sleeplessness, bugs, and last-minute document changes are part of the process. Persisting through these hurdles is what differentiates teams that finish from those that drop out.
4. **Be resourceful:** Adapt quickly, combine ideas, and leverage existing AI and AWS cloud services to accelerate development in a short timeframe.

{{% notice info "Conclusion" %}}
A Hackathon is not just a coding contest, but a realistic simulation of product development under extreme pressure. For students and industry newcomers, it is a golden opportunity to push boundaries, learn technical tools, and master essential soft skills.
{{% /notice %}}

---

### Part 3: Why We Always Need Confidence (Speaker: Nguyen Thi Quynh Nhu)

#### 1. Let's Talk Confidence – What is True Confidence?
The speaker opened the session with a famous quote by Mark Twain:
> *"Courage is resistance to fear, mastery of fear—not absence of fear."*

* **Key Message:** People often mistake confident people as having no fear. In reality:
  * Everyone has fears.
  * Everyone experiences moments of self-doubt.
  * True confidence is the **courage to act** despite feeling anxious and afraid.

#### 2. The Cost of Self-Doubt in Student Life
Behind the normal exterior of many students lie invisible barriers that hinder their growth:
* **Missed Chances:**
  * Knowing the correct answer in class but hesitating to raise a hand.
  * Having a unique idea during group discussions but choosing to stay silent.
  * Spotting great internships or competitions but holding back, thinking *"I am not good enough"*.
  * *Fear factor:* Fear of being wrong, being judged, being rejected, or being the center of attention. Consequently, opportunities disappear before one can act.
* **Voices Suppressed by Low Self-Esteem:**
  * Spotting an error in a project or having suggestions to improve a group presentation, but keeping quiet.
  * Constantly plagued by questions: *"Should I say it?"*, *"What if I'm wrong?"*, *"What will others think of me?"*.
  * This silence leads to many great ideas being buried.
* **Invisible Pressure & Hiding Potential:**
  * Others might only see a quiet, introverted student, but inside, they may be dealing with low self-esteem, anxiety, overthinking, and fear of judgment.
  * Many talented students fail to unleash their skills because they dread failure, leaving their potential permanently hidden.

{{% notice info "Memorable Takeaway" %}}
A lack of confidence not only robs individuals of opportunities to express themselves, but also deprives the team and the world of the ideas and values they could contribute.
{{% /notice %}}

#### 3. The Science Behind Low Self-Esteem
* **Fixed vs. Growth Mindset:** The speaker cited Carol Dweck's perspective:
  > *"Why waste time proving over and over how great you are when you could be getting better?"*
  * Low confidence often stems from a fixed mindset where mistakes and failures are viewed as direct measurements of self-worth.
  * Conversely, those with a growth mindset see failure as a learning experience and an opportunity to improve.
* **Imposter Syndrome:**
  * A psychological state widely experienced by students, fresh graduates, and especially tech professionals.
  * Despite having solid achievements, individuals feel inadequate, attribute success to mere luck, and constantly fear being exposed as a "fraud".

#### 4. The Power of Confidence
* **Confidence Helps People Connect:** Confidence helps us build and expand social relationships. Confident people communicate more openly, collaborate more effectively, and proactively connect with others.
* **In Academic & Professional Environments:** It helps present ideas articulately, participate actively in debates, enhance leadership skills, and expand professional networks.

#### 5. "Hacking" Confidence Daily
* **The 5-Second Rule:**
  * Formula: **5 → 4 → 3 → 2 → 1 → Action!**
  * *Application:* Whenever an opportunity arises and your brain starts generating doubts (e.g., *"I can't do this"*), count down and act immediately before self-doubt takes control.
  * *Real-world examples:* Raising your hand to ask a question, initiating a conversation with a stranger, signing up for a Hackathon, or submitting a resume for a dream job.

#### 6. Conclusion
The speaker concluded with an inspiring quote by Brené Brown:
> *"Vulnerability sounds like truth and feels like courage."*

{{% notice tip "Core Takeaway" %}}
Confidence does not mean being perfect or fearless. Confidence means **daring to act** even while feeling anxious. Every step outside your comfort zone builds lasting self-confidence. Don't wait until you feel confident to act; act to become confident!
{{% /notice %}}

---

### Part 4: Building "Tu Vi Dai Viet" Website by Yourself – From AI Concept to Production (Speaker: Nghia Tran)

#### 1. Product Concept & Market Opportunity
Unlike other purely technical AWS presentations, Mr. Nghia Tran’s session shared the real-world journey of a technology startup: building and running the AI-powered astrology website [Tu Vi Dai Viet](https://tuvidaiviet.com/).
* **Market Demand:**
  * Young people are highly curious about astrology and self-discovery, but face major barriers in traditional methods.
  * *High Consulting Fees:* Traditional high-quality astrology sessions cost from 500,000 VND to several million VND.
  * *Hard-to-Access Content:* Traditional texts use complex Sino-Vietnamese terminology, making them hard for youth to comprehend.
* **Tu Vi Dai Viet’s Goal:** Digitizing astrology to decode personal destinies (personality, relationships, careers) in a fast, intuitive, and accessible way for the younger generation under the message: *"Decoding destiny – Understanding your inner self"*.

#### 2. Unique Hybrid Model: AI + Expert Review
This is the highlight of the project's product thinking:
> [!NOTE]
> The development team **did not choose** to simply feed raw prompts into ChatGPT and return the results directly to the user.

Instead, they implemented a strict quality control process:
* **Testing and Evaluating Multiple AI Models:** The team continuously tested different Foundation Models to compare response quality, accuracy, and natural language style to choose the most suitable model for each task (similar to model comparison on Amazon Bedrock).
* **Expert Content Moderation (Human-in-the-loop):** After the generative AI creates the charts and interpretations, human astrology experts review the content, refine the terminology, and ensure maximum real-world value before it is shown to users.

#### 3. Business Model
The project adopts a **Freemium** model to attract traffic while generating sustainable revenue:
* **Free Tier:** Allows users to generate personal charts and view basic analytical summaries.
* **Premium Tier:** Unlocks detailed reports, step-by-step phase predictions, chart comparisons, and highly personalized in-depth analyses.

#### 4. Operational Journey and Migration to AWS
As the user base grew rapidly, the startup's challenge shifted from *"How does AI answer?"* to *"How to keep the system running stably?"*. The team had to optimize various operational layers such as hosting, database management, payment integration, email automation, monitoring, background jobs, and CI/CD pipelines.

To enhance scalability and reliability, the team planned a migration to AWS:
* **Initial Tech Stack:** Next.js, TypeScript, React, MySQL, and Prisma ORM.
* **Target AWS Architecture:**
  * **Amazon ECS (Elastic Container Service):** Run and manage application containers flexibly.
  * **Amazon RDS:** Managed relational database ensuring high performance and stability.
  * **Amazon Bedrock:** Platform for unified connection and management of Generative AI models.
  * **AWS Lambda & Amazon ElastiCache:** Optimize asynchronous processing and caching to reduce database loads.
  * **Amazon CloudWatch:** Monitor performance and detect system errors in real time.

{{% notice info "Core Takeaways" %}}
* **AI is only one part of the product:** A successful AI startup requires not just a good AI model, but also optimized user experience, stable operations, and strict quality control.
* **Always verify AI outputs:** Testing multiple models and incorporating expert verification minimizes AI hallucination risks.
* **Practical Product Mindset:** The key difference between **building an AI demo** and **operating a real AI product** lies in simultaneously solving technical, infrastructure, cost, business model, and user experience challenges in the real world.
{{% /notice %}}

---

### Part 5: The Hidden Iceberg of a Project: DevOps Before Disaster (Speaker: Tran Minh Quan)

#### 1. Session Overview & The Iceberg Analogy
The presentation utilized the iceberg analogy to describe a familiar but painful reality in software development: the issues we observe in projects are usually just the tip of the iceberg.
* **Visible Symptoms:** Deployment failures, missed deadlines, customer complaints, and team burnout are not the root causes, but rather the visible results of flaws embedded early in the development lifecycle.
* **Key Message:** DevOps is not merely a set of deployment tools or a CI/CD pipeline, but a comprehensive cultural and technical approach that detects and resolves issues before they escalate into disasters.

#### 2. The Hidden Cost of a "Simple Feature"
The speaker introduced a classic scenario: A feature estimated to take only two days of development, which seems straightforward. In reality:
* Requirements constantly change mid-way.
* Major misunderstandings occur among stakeholders (Client, Product, Dev).
* The team performs endless reworks, significantly exceeding the original schedule.
* Ultimately, the entire team experiences fatigue and exhaustion over a seemingly minor feature.

> [!IMPORTANT]
> **Key Lesson:** A "simple" feature is rarely simple. The greatest cost in software development does not lie in coding, but in communication, requirement clarification, testing, and cross-team collaboration.

#### 3. The Anatomy of a Project Iceberg: Surface vs. Subsurface

| Iceberg Layer | Symptoms / Root Causes | Operational Impact |
| :--- | :--- | :--- |
| **The Tip (Visible Symptoms)** | • Missed deadlines<br>• Production environment bugs<br>• Failed deployments<br>• Customer complaints<br>• Team burnout | Teams often rush to fix the immediate aftermath (patching bugs, working overtime, pushing speed) while ignoring the root cause. |
| **Beneath the Surface (Hidden Causes)** | • **Requirement Ambiguity**<br>• **Communication Gaps**<br>• **Siloed Teams**<br>• **Lack of Ownership**<br>• **Manual Processes**<br>• **Slow Feedback Loops** | • Misalignment between clients, product owners, and developers.<br>• Broken flow of information between Dev, QA, and Ops.<br>• Inter-departmental blame games during incidents.<br>• Manual deployments and testing introduce errors; late bug detection spikes cost. |

#### 4. The True Role of DevOps
To prevent project disasters, DevOps serves as a lifeline built upon three core pillars:
* **Collaboration Culture:** Breaks down silos between Dev, QA, Operations, and Product teams, fostering shared ownership of the final product.
* **Automation:** Minimizes human errors and saves time by automating integration and deployment (CI/CD), testing (Automated Testing), and managing environments (Infrastructure as Code - IaC).
* **Fast Feedback Loops:** Integrates monitoring, logging, and alerting systems to detect anomalies early and facilitate continuous feedback.

{{% notice info "Core Takeaway" %}}
Missed deadlines, bugs, and customer complaints are merely symptoms, not the actual disease. To cure a project at its root, teams must:
1. Clarify and freeze requirements early.
2. Enhance multidirectional communication.
3. Build a culture of shared responsibility.
4. Automate workflows to shorten feedback loops.
{{% /notice %}}
