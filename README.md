# 🚀 Fuzzy: AI-Powered SAAS Automation Builder (Zapier Clone)

Fuzzy is a sophisticated workflow automation application that allows users to build and manage automated sequences, significantly reducing manual effort for business processes. This complex project showcases advanced technical mastery by building system integrations completely from scratch.

---

## 📌 Badges

<p>
  <img src="https://img.shields.io/badge/Next.js-14-black" />
  <img src="https://img.shields.io/badge/Runtime-Bun-ffdd00" />
  <img src="https://img.shields.io/badge/TypeScript-5-blue" />
  <img src="https://img.shields.io/badge/Database-Neon%20Postgres-4BFF87" />
  <img src="https://img.shields.io/badge/ORM-Prisma-2D3748" />
  <img src="https://img.shields.io/badge/Auth-Clerk-purple" />
  <img src="https://img.shields.io/badge/Workflow-ReactFlow-1E90FF" />
  <img src="https://img.shields.io/badge/Billing-Stripe-blueviolet" />
  <img src="https://img.shields.io/badge/UI-TailwindCSS-38BDF8" />
</p>

---

## ✨ Key Features & Technical Highlights

| Feature Category | Description | Source Reference |
| :--- | :--- | :--- |
| **Project Goal** | Fuzzy allows users to build automations, operating kind of like Zapier [1]. |
| **Custom Integrations** | Integrations with **Discord**, **Slack**, **Google Drive**, and **Notion** are built entirely from scratch, without the use of any integration library [2]. |
| **Core Framework** | Built using **Next.js 14** (App Router) and utilizes **Bun/npm** for the runtime environment [3]. |
| **Workflow Editor** | Features a beautiful editor where users can customize their entire flow [4]. The editor utilizes powerful features like centering, zooming, and **locking the canvas** to prevent accidental deletion [4]. |
| **Trigger Mechanism** | Workflows are required to have a trigger, which in this case is **Google Drive** [4]. The application must set up a watch channel to listen to changes from a specific folder [5, 6]. |
| **Dynamic Actions** | Actions focused on in the implementation include **Slack**, **Notion**, and **Discord** [7]. |
| **Templating & Chips**| Users can create custom templates for messages [5]. The application allows users to include **dynamic chips** that pull data, such as the file name, from the Google Drive trigger event [5, 8]. |
| **Authentication** | Secure user management is handled by **Clerk Authentication** [3, 9]. |
| **Database Sync** | Clerk **webhooks** are utilized to send events (like user creation) to a dedicated endpoint (`/api/clerk-webhook`), allowing for database synchronization [10-12]. |
| **Monetization Model** | Features a dedicated **Stripe billing page** [13, 14]. A **credit system** is implemented where the user's remaining credit decreases by one when an automation sequence fires [15, 16]. |
| **Database** | Uses **Neon Tech** (PostgreSQL) integrated via **Prisma** ORM [3, 17]. |
| **UI/Design** | Utilizes **Aceternity UI** components for parallax animations, infinite carousels, and particle effects [3]. Full support for **Light mode and Dark mode** theming is included [13]. |

---

## 🛠️ Technology Stack

| Category | Technologies Used | Notes |
| :--- | :--- | :--- |
| **Framework/Language** | **Next.js 14** (App Router), **TypeScript**, **Bun** (Runtime). | Utilizes Next.js Server Actions and Middleware [3]. |
| **Database & ORM** | **Neon Tech** (PostgreSQL), **Prisma**. | Neon Tech is used for the scalable PostgreSQL instance [3, 17]. |
| **Styling & Components**| **Tailwind CSS**, **Shadcn UI**, **Aceternity UI**. | Implements modern, complex visual components [3]. |
| **Authentication** | **Clerk Authentication**, custom **OAuth 2.0** flow. | Handles custom scopes for services like Google Drive [18, 19]. |
| **Workflow Engine** | **React Flow**. | Used for building the interactive drag-and-drop editor [3, 4]. |
| **Monetization** | **Stripe**. | Used for payment processing and managing subscription products [3, 20]. |
| **Scheduling/Delay** | **Cron Jobs**. | Used for scheduling delayed workflow steps ("Weight Node") [3, 21]. |
| **File Handling** | **Uploadcare** (File Uploads) [3]. | Used specifically for user profile picture uploads [22]. |
| **Local Testing** | **Ngrok**. | Essential for testing webhooks (Clerk, Google Drive Watch API) locally [3, 23]. |

---

## 🗺️ Architectural Workflow

```mermaid
graph TD

    %% High-contrast styles (GitHub-safe)
    classDef blue fill:#1E3A8A,color:#FFF,stroke:#000,stroke-width:1px;
    classDef pink fill:#9D174D,color:#FFF,stroke:#000,stroke-width:1px;
    classDef yellow fill:#CA8A04,color:#FFF,stroke:#000,stroke-width:1px;
    classDef lightblue fill:#0369A1,color:#FFF,stroke:#000,stroke-width:1px;
    classDef lavender fill:#5B21B6,color:#FFF,stroke:#000,stroke-width:1px;
    classDef purple fill:#4C1D95,color:#FFF,stroke:#000,stroke-width:1px;
    classDef gray fill:#374151,color:#FFF,stroke:#000,stroke-width:1px;
    classDef green fill:#065F46,color:#FFF,stroke:#000,stroke-width:1px;

    %% USER ONBOARDING
    subgraph User_Onboarding
        U[User logs in] --> W1[Clerk Webhook: api-clerk-webhook]
        W1 --> DB[(Database Neon Prisma)]
        U --> A1[Connections Page OAuth Setup]
        A1 --> DB
    end

    %% AUTOMATION TRIGGER
    subgraph Automation_Trigger
        WFA[Workflow Published] --> S1[Google Drive Watch Request]
        S1 --> C1[Drive Activity Webhook]
    end

    %% EXECUTION FLOW
    subgraph Execution_Flow
        C1 --> C2[Check Credits]
        C2 --> E1[Deduct 1 Credit]
        E1 --> D1[Run Node Sequence]

        D1 --> D2[Action Node Slack Notion Discord]
        D1 --> D3[Delay Node Cron Schedule]

        D2 --> D4[External API Call]
        D4 --> E2[Automation Complete]

        D3 --> E1
    end

    %% DATA STORES
    subgraph Data_Stores
        DB -.-> Tokens[OAuth Tokens]
        DB -.-> Workflow[Node Templates]
        DB -.-> User[User Tier and Credits]
    end

    %% Apply styles
    class U,DB blue
    class W1 pink
    class A1 lightblue
    class C1 lavender
    class C2 purple
    class D1,D3 gray
    class D2,D4 green

