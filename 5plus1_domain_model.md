# 5+1 Domain Model – Complete Recap

**Context**: Foundational, silo-free model for any business.  
**Scoped example**: Fitness facility offering memberships, coached classes, schedule reservations, and retail (apparel, supplements, beverages).  
**Version**: 1.0 (recap of conversation)

---

## Core Philosophy

These definitions are written at the **root level**—foundational, timeless, and deliberately stripped of today’s common departmental silos, tools, or buzzwords.  
Each domain is a complete, self-contained sphere of responsibility that any business (product, service, physical, digital, or hybrid) must own end-to-end.

**Separation logic**:
- Customer = everything the buyer feels and receives
- Supplier = everything the provider feels and delivers inbound
- Operations = the internal engine that turns supplier inputs into customer outputs
- Finance = the economic heartbeat and guardrails
- Sales & Marketing = the demand-generation and conversion front door
- Infrastructure = the invisible platform everything else runs on

This model is intentionally symmetrical and exhaustive: every activity, decision, or asset falls cleanly into exactly one of these six domains. No overlaps, no orphaned responsibilities.

Domains can be owned by different organizations (outsourced) while remaining loosely coupled via events/messages.

---

## 1. Customer Experience Domain (Customer Domain)

**Core focus**: Every single interaction an individual or organization has with the business—from first awareness through loyalty, advocacy, or churn.

### Key Responsibilities
- Owning the full customer journey and perception of the brand/business
- Designing, measuring, and continuously improving every touchpoint
- Capturing voice-of-customer feedback and turning it into actionable changes
- Ensuring delight, trust, retention, and advocacy
- Managing post-purchase/support relationships and lifecycle value

### Typical Activities / Teams
- Customer success / account management
- Customer support / service / helpdesk / chat / ticketing
- Onboarding, adoption, education, and usage guidance
- Customer feedback programs (NPS, CSAT, surveys, interviews)
- Experience design (journey mapping, service blueprints)
- Complaint resolution, escalation, and recovery
- Loyalty / retention programs, win-back efforts

### Fitness Facility Specific Ownership
- Website, ecommerce site, and POS (in-person & digital checkout)
- Member-facing calendar, reservations, cancellations, waitlists
- Electronic payment **execution** (charge initiation, retries, dunning, member-facing presentment of bills)
- Billing rule engine (prorations, freezes, credits, no-show fees, promo application)
- Automated member communications / notifications (reminders, confirmations, no-show alerts, payment failed, win-back)
- Basic retention / churn signals and interventions
- Attendance impact (applying no-show fees/credits, post-check-in messaging)
- Member profile as single source of truth (deduplication, activation rules)

### Boundary Examples
- Belongs here: Handling a support ticket after purchase  
  Does **not** belong here: Running paid ads to acquire new leads (Demand Creation)
- Belongs here: Personalizing the product usage experience  
  Does **not** belong here: Actually building the product feature (Operations)
- Belongs here: Generating and presenting the member’s invoice and initiating the charge  
  Does **not** belong here: Booking the general-ledger entry or bank reconciliation (Finance)

---

## 2. Supplier Experience Domain (Supplier Domain)

**Core focus**: Every single interaction the business has with any entity that supplies inputs—raw materials, components, services, talent, data, or capital.

### Key Responsibilities
- Sourcing, selecting, onboarding, and maintaining supplier relationships
- Negotiating terms, contracts, pricing, SLAs, and performance expectations
- Managing supplier risk, compliance, diversity, and sustainability
- Ensuring reliable, timely, high-quality inbound flow of inputs
- Optimizing total cost of ownership and supplier innovation

### Typical Activities / Teams
- Strategic sourcing / procurement / vendor management
- Supplier relationship management (SRM)
- Contract negotiation and administration
- Supplier onboarding, qualification, and audits
- Supplier performance scorecards and development programs
- Inbound logistics coordination
- Paying suppliers (execution of payment)

### Fitness Facility Specific Ownership
- Apparel, supplements, and beverage vendors
- Equipment suppliers
- Reacting to low-stock signals from Operations and creating purchase orders
- Receiving deliveries and confirming quality

### Boundary Examples
- Belongs here: Negotiating volume discounts with a raw material vendor  
  Does **not** belong here: Paying the invoice on time (Finance) or managing physical stock levels (Operations)
- Belongs here: Auditing a freelance talent platform’s compliance  
  Does **not** belong here: Actually scheduling that person into a class (Operations)

---

## 3. Value Delivery Domain (Operations Domain)

**Core focus**: The core transformation engine—taking inputs secured by Supplier and converting them into the exact products or services that Customer consumes.

### Key Responsibilities
- Designing, running, and improving the core production/delivery processes
- Managing capacity, quality, speed, cost, and reliability of output
- Coordinating internal workflows, resources, and hand-offs
- Fulfillment, assembly, manufacturing, service delivery, or software deployment
- Continuous process optimization and innovation in how value is created

### Typical Activities / Teams
- Production / manufacturing / assembly lines
- Service delivery / field operations / installation
- Software development, DevOps, release management (product creation)
- Order fulfillment, picking, packing, shipping
- Inventory management, warehouse operations
- Quality assurance / control / testing
- Internal supply chain coordination (after supplier hand-off)

### Fitness Facility Specific Ownership
- Class scheduling (recurring rules, e.g., M-F at 6/7/8/9/12/4/5/6 for 1 hour)
- Generation of future class instances (e.g., 3 months out)
- Coach (instructor) scheduling and assignment
  - Default coach per recurring template
  - Coach self-service sub request + mandatory Ops approval
  - Invariant: Once assigned, the class remains the coach’s responsibility until a replacement is formally approved
- Physical inventory tracking, locations, shrinkage, quality/expiration
- Low-stock detection → publish `InventoryThresholdReached`
- Attendance / check-in capture (QR, app, staff mark)
- Publishing factual delivery events (`AttendanceRecorded`, `NoShowDetected`, `ClassInstancesGenerated`, `InstructorAssignmentChanged`)
- Retail fulfillment (picking/packing physical items)

### Boundary Examples
- Belongs here: Building and testing a new software feature  
  Does **not** belong here: Marketing that feature to prospects (Demand Creation)
- Belongs here: Packing and shipping the customer’s order  
  Does **not** belong here: Handling a delivery complaint (Customer Experience)
- Belongs here: Creating the master class schedule and assigning coaches  
  Does **not** belong here: Presenting the calendar to members or letting them book (Customer Experience)

---

## 4. Value Stewardship Domain (Finance Domain)

**Core focus**: The complete flow and stewardship of all monetary and economic resources across the entire business.

### Key Responsibilities
- Overall cash management, liquidity, and working capital
- Financial planning, budgeting, forecasting, and scenario modeling
- Accounting, bookkeeping, financial reporting, and compliance
- Capital allocation, investment decisions, funding strategy
- Risk management (financial/credit/insurance), pricing economics
- Performance measurement (profitability, ROI, unit economics)

### Typical Activities / Teams
- FP&A (financial planning & analysis)
- Accounting / controllership / audit
- Treasury / cash management
- Tax strategy and compliance
- Investor relations / capital raising
- Cost accounting / product profitability analysis
- Payment processing (outbound to suppliers, inbound reconciliation)

### Fitness Facility Specific Ownership
- General-ledger / bank-account level only
- Receiving summarized events (`ChargeSucceeded`, `ChargeFailed`, `RefundIssued`) with net amount, category, and reference
- Booking journal entries, revenue recognition (including deferrals), reconciliation
- **Does not** own bill generation, presentment, or charge initiation (those live in Customer Experience because of rule complexity)

### Boundary Examples
- Belongs here: Deciding the overall pricing strategy economics  
  Does **not** belong here: Promoting a promotional price to attract leads (Demand Creation)
- Belongs here: Approving capital expenditure for new equipment  
  Does **not** belong here: Operating that equipment (Operations)
- Belongs here: Reconciling the bank deposit and posting the GL entry  
  Does **not** belong here: Generating the member’s invoice or deciding the proration (Customer Experience)

---

## 5. Demand Creation & Conversion Domain (Sales & Marketing Domain)

**Core focus**: The concrete activities of identifying, attracting, and converting potential customers into actual paying customers.

### Key Responsibilities
- Building awareness, interest, and desire in the market
- Generating and qualifying leads / opportunities
- Nurturing prospects through the funnel
- Closing sales / securing commitments / contracts
- Brand positioning, messaging, and demand-generation campaigns

### Typical Activities / Teams
- Brand marketing / advertising / content marketing
- Digital marketing (SEO, PPC, social, email campaigns)
- Lead generation / inbound marketing
- Sales development / outbound prospecting
- Account-based marketing / sales
- Direct sales, channel / partner sales, ecommerce checkout (acquisition side)
- Pricing promotions / sales incentives (tactical execution)

### Fitness Facility Specific Ownership
- Market sensing and product selection decisions (which apparel, supplements, or new class formats to offer)
- Defining product attributes (name, description, images, variants, list pricing, promotional pricing strategy)
- Publishing product information and campaign creative
- Event / pop-up signups using their own lightweight tools
- Publishing provisional member records (`ProvisionalMemberCreated` / `NewSignupFromEvent`)
- Targeted win-back campaigns triggered by churn-risk signals

### Boundary Examples
- Belongs here: Running a Google Ads campaign for new leads  
  Does **not** belong here: Delivering the product after the sale (Operations)
- Belongs here: Closing the deal and signing the contract  
  Does **not** belong here: Onboarding the new customer post-sale (Customer Experience)
- Belongs here: Deciding to launch a new protein flavor and writing the product copy  
  Does **not** belong here: Making the SKU shoppable in the member app or managing stock levels (Customer Experience / Operations)

---

## 6. Enabling Foundation Domain (Infrastructure Domain) – the +1

**Core focus**: The shared, non-negotiable platform that makes all five other domains possible and safe to operate.

### Key Responsibilities
- Providing secure, reliable, scalable foundational systems and capabilities
- Ensuring legal, regulatory, safety, security, and ethical compliance
- Maintaining physical/digital facilities, tools, and shared services
- Protecting the business (cyber, physical, data, IP)
- Enabling connectivity, data flow, and interoperability

### Typical Activities / Teams
- IT infrastructure / cloud / networks / cybersecurity
- Facilities management / office / data centers
- Legal, compliance, governance, privacy (GDPR, etc.)
- Information security / risk & audit (non-financial)
- HR systems & payroll infrastructure (but not talent acquisition)
- Enterprise architecture, shared data platforms
- Business continuity, disaster recovery

### Fitness Facility Specific Ownership
- Gym management software platform, event bus / webhooks / APIs
- Access control (RFID, door systems)
- Digital waiver storage and compliance
- Payment gateway / PCI-compliant tokenization infrastructure
- Shared catalog and integration layer between domains
- Security, backups, system uptime

### Boundary Examples
- Belongs here: Running the CRM platform and its uptime  
  Does **not** belong here: Using the CRM to manage sales pipelines (Demand Creation)
- Belongs here: Ensuring data privacy compliance across all domains  
  Does **not** belong here: Collecting customer feedback (Customer Experience)

---

## Common Inter-Domain Messages (Fitness Context)

Most frequent everyday events:

| Event | Published By | Consumed By | Purpose |
|-------|--------------|-------------|---------|
| `ProvisionalMemberCreated` / `NewSignupFromEvent` | Demand Creation | Customer Experience | Hand off new signup for rules & activation |
| `MemberActivated` | Customer Experience | Operations, Finance | Member is live |
| `ClassInstancesGenerated` | Operations | Customer Experience | Build member calendar |
| `ClassReservationMade` | Customer Experience | Operations | Reserve capacity |
| `AttendanceRecorded` / `NoShowDetected` | Operations | Customer Experience | Apply consequences & notifications |
| `InstructorAssignmentChanged` | Operations | Customer Experience | Update member view |
| `ChargeSucceeded` / `ChargeFailed` / `RefundIssued` | Customer Experience | Finance | GL posting & reconciliation |
| `InventoryThresholdReached` | Operations | Supplier Experience | Trigger reorder |
| `RetailOrderPlaced` | Customer Experience | Operations + Finance | Fulfill & book revenue |
| `ChurnRiskDetected` | Customer Experience | Demand Creation | Trigger win-back |

---

## Key Design Decisions (Fitness Facility)

1. **Customer Experience owns payment execution** (because nearly all payments are electronic and the member-facing experience of billing is complex).
2. **Finance stays at GL / bank-account level** only—receives summarized events.
3. **Operations owns class scheduling + coach assignment** (including the self-service sub-request with mandatory Ops approval and the “coach remains responsible until approved” invariant).
4. **Customer Experience owns the member calendar and reservation experience** (read from Operations events).
5. **Sales & Marketing can use lightweight event tools** and publish provisional records; Customer Experience applies authoritative rules.
6. **Operations owns physical inventory truth**; Customer Experience owns digital availability display.
7. **MVP must include**: attendance/check-in, automated notifications, and basic churn signals (in addition to the core signup → schedule → reserve → bill loop).

---

## Business-Level Contracts (No Code)

Contracts are written in plain business language before any implementation. Example structure:

```markdown
# Contract: [Name]

**Participating Domains**
- Publisher
- Subscriber(s)

**Published Events**
1. `EventName`
   - Trigger: …
   - Payload (minimal fields): …
   - Invariants: …

**Business Rules / Invariants**
- …
```

These contracts enable parallel work, outsourcing, and clear accountability while keeping domains loosely coupled.

---

*End of recap.*  
This document captures the foundational definitions plus all refinements made for the fitness facility example.
