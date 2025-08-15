Thank you for the detailed input. Below is a **well-formatted requirements document** based on your specifications, structured as a professional `requirements.md` file. This includes:

- Project overview
- Functional & non-functional requirements
- Technical stack recommendations
- Database selection justification
- Angular concepts to be demonstrated
- Portfolio best practices
- Modular role support
- Versioning and admin features
- Test considerations

---

# 📄 Portfolio Website Requirements (`requirements.md`)

> **Project Name**: Full-Stack Resume & Portfolio Builder (MEAN-inspired with Free Cloud Backend)  
> **Target Audience**: Developers, Architects, Cloud Engineers  
> **Hosting**: GitHub Pages (frontend), Free Cloud Database (backend)  
> **Status**: Requirements Specification v1.0

---

## 🎯 Project Overview

Build a **full-stack, responsive portfolio website** using **Angular** as the frontend framework, with dynamic resume-building capabilities. The site will have **public and admin views**, where:

- **Public users** can view published portfolios and resumes.
- **Admin users** can log in, create, edit, version, and publish content dynamically.
- All data is stored in a **free, JSON-capable cloud database**.
- The resume page should render like a **Microsoft Word document**, with modular, reusable Angular components.
- Support for **role-based resume styling** (e.g., Backend Developer, Cloud Engineer).
- Resume sections are editable, saveable, and version-controlled.

---

## 🔧 Technical Stack

| Layer        | Technology                     | Justification |
|-------------|-------------------------------|-------------|
| **Frontend** | Angular (latest LTS: v17+)     | Component-based, strong typing, CLI tooling, Material Design support |
| **UI Framework** | Angular Material             | Pre-built accessible components, theming, responsive design |
| **Backend**  | Express.js (Node.js)           | Lightweight, integrates well with Angular, easy REST API creation |
| **Database** | **Supabase (Recommended)**     | Free PostgreSQL with JSONB, REST + Realtime APIs, Auth, generous free tier |
| **Alternative DBs** | Firebase Firestore, MongoDB Atlas, Neon.tech | See [Database Comparison](#database-comparison) |
| **Hosting**  | GitHub Pages (frontend)        | Free, CDN-backed, easy CI/CD via GitHub Actions |
| **Auth**     | Supabase Auth / Firebase Auth  | Built-in email/password, social login, session management |

---

## 🗃️ Database Comparison & Recommendation

| Option | JSON Support | Free Forever? | REST API | Notes |
|-------|--------------|---------------|---------|-------|
| **Supabase** ✅ | Yes (PostgreSQL JSONB) | Yes (500MB DB, 1GB storage) | Auto-generated | Auth, Realtime, Row-level security |
| **Firebase Firestore** | Yes (NoSQL JSON docs) | Yes (1GB storage, limited ops/day) | Yes (via SDK/REST) | Google-backed, easy auth, but scales cost fast |
| **MongoDB Atlas** | Yes (Native JSON) | Yes (512MB shared cluster) | Manual or via Stitch | Requires custom API layer |
| **Neon.tech** | Yes (PostgreSQL JSONB) | Yes (3GB storage) | Manual (Express.js) | Branching, serverless, great for dev |

> ✅ **Recommendation: Supabase**  
> - Best balance of **free tier**, **JSON support**, **REST API**, **authentication**, and **scalability**.
> - PostgreSQL allows relational + document flexibility.
> - Integrates easily with Express or directly from Angular (via Supabase JS client).

---

## 🌐 Functional Requirements

### 1. **Public View**
- Users can view the portfolio and resume pages.
- Responsive design across devices.
- Public preview of the latest **published version** of the resume.
- No login required.

### 2. **Admin View**
- Secure login (email/password) via Supabase Auth.
- After login, access to **edit mode**.
- Ability to:
  - Create, read, update, delete (CRUD) pages.
  - Add, edit, or remove content blocks (text, lists, skills, etc.).
  - Add and manage a **to-do list** per user.
  - Upload a resume (PDF/DOCX) and **parse sections** (via backend parser).
  - Preview changes before publishing.

### 3. **Resume Builder**
- Resume rendered as a **single-page document** styled like MS Word.
- Sections as **reusable Angular components**:
  - `PersonalInfoComponent`
  - `ExperienceComponent`
  - `EducationComponent`
  - `SkillsComponent`
  - `ProjectsComponent`
  - `CertificationsComponent`
- Each section pulls data from **JSON via REST API**.
- Support for **multiple resume styles** (e.g., chronological, functional, hybrid).
- **Role-based filtering**: Highlight sections based on selected role:
  - Backend Developer (Java, Node.js, Python)
  - Frontend (React, Angular)
  - Cloud Engineer (AWS, GCP, Azure)
  - Solution Architect
  - Microservices Developer

### 4. **Versioning & Publishing**
- Track changes to resume and pages.
- Save drafts and **create multiple versions**.
- View **version history** and **revert** to any previous version.
- **Publish** button to make changes live in public view.
- Published version is immutable until next publish.

### 5. **Dynamic Pages & Blocks**
- Admin can:
  - Add new pages (e.g., "Blog", "Projects", "Contact").
  - Edit existing pages (except standard ones like "Home", "Resume" — these can be modified but **not deleted**).
  - Create reusable **content blocks** (text, image, list, code snippet).
  - Drag-and-drop or reorder blocks (future enhancement).

### 6. **To-Do List**
- Personal to-do list stored per user.
- CRUD operations.
- Persists across sessions.

### 7. **Document Upload & Parsing**
- Admin can upload a resume (PDF or DOCX).
- Backend parses the document (using libraries like `pdf-parse`, `mammoth`).
- Extracts sections (Experience, Skills, etc.) and maps to JSON.
- Generates a similar-style resume in the editor.

---

## 🧩 Angular Concepts to Demonstrate

| Concept | Implementation |
|--------|----------------|
| **Component Lifecycle Hooks** | Use `ngOnInit()` to fetch data, `ngOnDestroy()` to clean subscriptions, `ngAfterViewInit()` for DOM manipulation in resume preview |
| **Session Management** | Use Supabase Auth tokens (JWT), store in memory (not localStorage for security), interceptors to handle auth state |
| **Reactive Forms** | For editing resume sections and pages |
| **Services & Dependency Injection** | Data service to fetch JSON from API, shared across components |
| **Routing** | Public vs. Admin routes with guards (`CanActivate`) |
| **Observables & RxJS** | Real-time updates, API calls, form changes |
| **Angular Material** | Buttons, cards, dialogs, form fields, theming |
| **Lazy Loading** | Split admin module for performance |
| **Change Detection Strategy** | Use `OnPush` for performance in reusable components |
| **Latest Angular LTS Features (v17+)** | 
| - **Standalone Components** | No more `NgModule`, cleaner architecture |
| - **Deferrable Views** (`@defer`) | Lazy-load resume sections |
| - **Control Flow Syntax** (`@if`, `@for`, `@switch`) | Replace *ngIf/*ngFor |
| - **Server-Side Rendering (SSR) via Angular Universal** | Optional for SEO |

---

## 🛠️ Data Model (Supabase Tables)

```sql
-- Pages
pages (
  id UUID PRIMARY KEY,
  title TEXT,
  is_standard BOOLEAN DEFAULT FALSE,
  content JSONB,
  created_at TIMESTAMPTZ,
  updated_at TIMESTAMPTZ
)

-- Resume Versions
resume_versions (
  id UUID PRIMARY KEY,
  user_id TEXT,
  version_number INT,
  data JSONB,
  created_at TIMESTAMPTZ,
  is_published BOOLEAN DEFAULT FALSE
)

-- Blocks
blocks (
  id UUID PRIMARY KEY,
  page_id UUID,
  type TEXT, -- 'text', 'list', 'skill-grid'
  content JSONB,
  order INT
)

-- To-Do Lists
todos (
  id UUID PRIMARY KEY,
  user_id TEXT,
  items JSONB, -- [{text, completed}]
  created_at TIMESTAMPTZ
)

-- Users (handled by Supabase Auth)
auth.users
```

---

## 🧪 Testing Requirements

| Test Type | Scope | Tools |
|---------|-------|-------|
| **Unit Tests** | Components, services, pipes | Jasmine + Karma |
| **Integration Tests** | Form submissions, API calls | TestBed, HttpTestingController |
| **End-to-End (E2E)** | User flows (login, edit, publish) | Cypress or Playwright |
| **Accessibility** | WCAG compliance | axe-core, Lighthouse |
| **Performance** | Load time, bundle size | Lighthouse, Webpack Bundle Analyzer |

---

## 🏗️ Development Roadmap (Incremental Steps)

### Phase 1: Static Portfolio (Week 1)
- Set up Angular app with Material Design.
- Create static pages: Home, Resume, About, Contact.
- Host on GitHub Pages.

### Phase 2: Dynamic Resume (Week 2)
- Convert resume into reusable components.
- Fetch resume data from local JSON.
- Add role-based filtering.

### Phase 3: Backend Integration (Week 3)
- Set up Supabase.
- Create tables and REST API.
- Connect Angular to Supabase via services.

### Phase 4: Admin & Auth (Week 4)
- Add login using Supabase Auth.
- Implement session management.
- Build admin dashboard.

### Phase 5: Versioning & Publishing (Week 5)
- Add version history.
- Implement publish/unpublish.
- Revert functionality.

### Phase 6: Upload & Parsing (Week 6)
- Add file upload.
- Parse PDF/DOCX and extract resume data.
- Auto-generate resume in editor.

### Phase 7: To-Do & Blocks (Week 7)
- Add dynamic blocks.
- Implement to-do list.

### Phase 8: Testing & Optimization (Week 8)
- Write unit and E2E tests.
- Optimize performance.
- Deploy full stack.

---

## 🎨 Portfolio Best Practices

- **Mobile-first responsive design**
- **Fast loading** (lazy load images, defer non-critical components)
- **SEO-friendly** (meta tags, SSR optional)
- **Accessibility** (ARIA labels, keyboard navigation)
- **Clean, professional typography**
- **Consistent color scheme and spacing**
- **Error handling and loading states**
- **Version-controlled content**

---

## 💡 Future Enhancements (Optional)

- Export resume as PDF.
- AI-powered resume suggestions.
- Multi-user support (for teams).
- Themes (light/dark mode).
- Blog integration.
- CI/CD pipeline for auto-deploy.

---

## 📂 Project Structure Suggestion

```
/portfolio-app
├── src/
│   ├── app/
│   │   ├── components/
│   │   │   ├── resume/
│   │   │   │   ├── personal-info/
│   │   │   │   ├── experience/
│   │   │   │   └── skills/
│   │   │   ├── layout/
│   │   │   └── shared/
│   │   ├── pages/
│   │   │   ├── public/
│   │   │   └── admin/
│   │   ├── services/
│   │   │   ├── data.service.ts
│   │   │   └── auth.service.ts
│   │   ├── models/
│   │   └── app.routes.ts
│   ├── assets/
│   │   └── resumes/ (sample JSON)
│   └── index.html
├── backend/
│   └── server.js (Express + Supabase)
├── tests/
├── requirements.md
└── README.md
```

---

## ✅ Next Steps

1. **Set up Angular project**:
   ```bash
   ng new portfolio-app --standalone --routing=true --style=scss
   cd portfolio-app
   ng add @angular/material
   ```

2. **Create Supabase project**:
   - Go to [supabase.com](https://supabase.com)
   - Create project, note API URL and anon key
   - Set up tables as defined above

3. **Initialize GitHub repo** and enable GitHub Pages.

4. **Begin Phase 1** — Build static portfolio.

---

> ✅ **You’re all set to start building!**  
> This document serves as your blueprint. You can enhance it anytime by adding features to `requirements.md`.

Let me know when you're ready to proceed to **Step 1: Setting up the Angular Project**, and I’ll guide you through the initial code structure and component setup.
