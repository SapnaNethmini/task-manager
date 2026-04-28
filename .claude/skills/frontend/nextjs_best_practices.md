# Next.js Best Practices

Guides the development of high-performance web applications using the Next.js App Router architecture and React Server Components.

---

## Skill Info

**Name:** nextjs-best-practices
**Description:** Next.js App Router principles. Server Components, data fetching, routing patterns.
**Risk:** unknown
**Source:** community
**Date Added:** 2026-02-27

---

## Overview

Next.js Best Practices focus on building scalable applications using:

* App Router architecture
* React Server Components
* Efficient data fetching strategies
* Proper routing structure
* Performance optimization techniques

---

## 1. Server vs Client Components

### Decision Tree

Does it need:

* State, effects, or event handlers → Client Component (`use client`)
* Direct data fetching without interactivity → Server Component (default)
* Both → Split into Server parent + Client child

### Default Usage

| Type   | Use Case                               |
| ------ | -------------------------------------- |
| Server | Data fetching, layouts, static content |
| Client | Forms, buttons, interactive UI         |

---

## 2. Data Fetching Patterns

### Fetch Strategies

| Pattern    | Use Case                      |
| ---------- | ----------------------------- |
| Default    | Static (cached at build time) |
| Revalidate | ISR (time-based refresh)      |
| No-store   | Dynamic (every request)       |

### Data Flow

| Source     | Pattern                       |
| ---------- | ----------------------------- |
| Database   | Server Component fetch        |
| API        | fetch with caching            |
| User input | Client state + server actions |

---

## 3. Routing Principles

### File Conventions

| File          | Purpose        |
| ------------- | -------------- |
| page.tsx      | Route UI       |
| layout.tsx    | Shared layout  |
| loading.tsx   | Loading state  |
| error.tsx     | Error boundary |
| not-found.tsx | 404 page       |

### Route Organization

| Pattern             | Use Case                       |
| ------------------- | ------------------------------ |
| Route groups (())   | Organize without affecting URL |
| Parallel routes (@) | Multiple pages at same level   |
| Intercepting routes | Modal overlays                 |

---

## 4. API Routes

### Route Handlers

| Method | Use         |
| ------ | ----------- |
| GET    | Read data   |
| POST   | Create data |
| PUT    | Update data |
| DELETE | Remove data |

### Best Practices

* Validate input with Zod
* Return proper HTTP status codes
* Handle errors gracefully
* Use Edge runtime when possible

---

## 5. Performance Principles

### Image Optimization

* Use `next/image`
* Set priority for above-the-fold images
* Use blur placeholders
* Use responsive image sizes

### Bundle Optimization

* Use dynamic imports for heavy components
* Rely on automatic route-based splitting
* Analyze bundle size when needed

---

## 6. Metadata

### Static vs Dynamic

| Type             | Use Case          |
| ---------------- | ----------------- |
| Static export    | Fixed metadata    |
| generateMetadata | Dynamic per route |

### Essential Tags

* title (50–60 characters)
* description (150–160 characters)
* Open Graph images
* Canonical URL

---

## 7. Caching Strategy

### Cache Layers

| Layer   | Control         |
| ------- | --------------- |
| Request | fetch options   |
| Data    | revalidate/tags |
| Route   | route config    |

### Revalidation

| Method     | Use Case                       |
| ---------- | ------------------------------ |
| Time-based | revalidate: 60                 |
| On-demand  | revalidatePath / revalidateTag |
| No cache   | no-store                       |

---

## 8. Server Actions

### Use Cases

* Form submissions
* Data mutations
* Trigger revalidation

### Best Practices

* Use `'use server'`
* Validate all inputs
* Return typed responses
* Handle errors properly

---

## 9. Anti-Patterns

| Don’t Do                   | Do Instead                 |
| -------------------------- | -------------------------- |
| Use client everywhere      | Server by default          |
| Fetch in client components | Fetch in server components |
| Skip loading states        | Use loading.tsx            |
| Ignore error boundaries    | Use error.tsx              |
| Large client bundles       | Use dynamic imports        |

---

## 10. Project Structure

app/
├── (marketing)/
│   └── page.tsx
├── (dashboard)/
│   ├── layout.tsx
│   └── page.tsx
├── api/
│   └── [resource]/
│       └── route.ts
└── components/
└── ui/

---

## Final Note

Server Components are the default in Next.js App Router. Start with server-first architecture and introduce client components only when necessary.
