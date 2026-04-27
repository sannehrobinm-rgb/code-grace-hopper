# 💻 Code Grace Hopper

> Plateforme pédagogique interactive — suivi de progression, cartes de compétences, cours en PDF et timeline d'apprentissage.

![Status](https://img.shields.io/badge/status-en%20développement-orange)
![Stack](https://img.shields.io/badge/stack-Next.js%20%7C%20TypeScript%20%7C%20Drizzle-blue)

---

## 🎯 Concept

Code Grace Hopper est une application fullstack à destination des apprenants en développement web. Elle centralise les cours (PDF), les projets, les compétences acquises et la progression, le tout présenté dans une interface immersive à base de cartes interactives (flip).

---

## ✨ Fonctionnalités

- 🃏 Cartes étudiants interactives (flip recto/verso)
- 📄 Visualisation des cours en PDF intégré
- 📈 Suivi de progression par compétence
- 🗂️ Gestion des projets liés aux étudiants
- 🔐 Authentification sécurisée (JWT)
- 🔒 Espace admin protégé

---

## 🛠️ Stack technique

| Couche | Technologie |
|--------|-------------|
| Framework | Next.js 14 (App Router) |
| Langage | TypeScript |
| UI | React + Tailwind CSS |
| Animations | Framer Motion (flip cards) |
| ORM | Drizzle ORM |
| Base de données | PostgreSQL (Neon) |
| Auth | JWT + bcrypt + cookies httpOnly |
| PDF | iframe / viewer intégré |
| Déploiement | Vercel |

---

## 🚀 Installation

```bash
git clone https://github.com/TON_USERNAME/code-grace-hopper.git
cd code-grace-hopper

npm install

cp .env.example .env.local
# → renseigner DATABASE_URL + JWT_SECRET

# Drizzle : pousser le schéma
npx drizzle-kit push

npm run dev
```

---

## 📁 Structure du projet

```
src/
├── app/
│   ├── api/
│   │   ├── auth/
│   │   ├── students/
│   │   ├── projects/
│   │   ├── skills/
│   │   └── progress/
│   ├── timeline/       # Route protégée
│   ├── admin/          # Route protégée
│   └── page.tsx
├── components/
│   ├── ui/
│   ├── layout/
│   └── cards/          # Flip cards
├── lib/
│   ├── db.ts           # Client Drizzle
│   └── auth.ts
├── types/
└── hooks/
drizzle/
└── schema.ts           # Schéma Drizzle
```

---

## 🗄️ Modèles de données (Drizzle)

```typescript
// drizzle/schema.ts
export const students = pgTable('students', {
  id: text('id').primaryKey().default(sql`gen_random_uuid()`),
  name: text('name').notNull(),
  email: text('email').unique().notNull(),
  cohort: text('cohort'),
  createdAt: timestamp('created_at').defaultNow(),
})

export const skills = pgTable('skills', {
  id: text('id').primaryKey().default(sql`gen_random_uuid()`),
  name: text('name').notNull(),
  category: text('category'),
})

export const progress = pgTable('progress', {
  id: text('id').primaryKey().default(sql`gen_random_uuid()`),
  studentId: text('student_id').references(() => students.id),
  skillId: text('skill_id').references(() => skills.id),
  level: integer('level').default(0), // 0-3
  updatedAt: timestamp('updated_at').defaultNow(),
})
```

---

## 📡 API Routes

| Méthode | Route | Description |
|---------|-------|-------------|
| POST | `/api/auth/login` | Connexion |
| GET | `/api/students` | Liste des étudiants |
| POST | `/api/students` | Ajouter un étudiant |
| GET | `/api/students/:id` | Profil complet |
| GET | `/api/skills` | Liste des compétences |
| PUT | `/api/progress` | Mettre à jour la progression |

---

## 🔐 Routes protégées

| Route | Accès |
|-------|-------|
| `/timeline` | Utilisateur connecté |
| `/admin` | Admin uniquement |

Protection via Middleware Next.js + vérification JWT.

---

## 👤 Auteur

Développé par **[TON NOM]** — projet de formation développeur web fullstack.
