<div align="center">

# ✍️  full-stack blogging

### A modern full-stack blogging platform built with React 19 & Express 5

[![React](https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev)
[![Express](https://img.shields.io/badge/Express-5.0-000000?style=for-the-badge&logo=express&logoColor=white)](https://expressjs.com)
[![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-47A248?style=for-the-badge&logo=mongodb&logoColor=white)](https://mongodb.com)
[![TailwindCSS](https://img.shields.io/badge/Tailwind-CSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)](https://tailwindcss.com)
[![Clerk](https://img.shields.io/badge/Clerk-Auth-6C47FF?style=for-the-badge&logo=clerk&logoColor=white)](https://clerk.com)

[Live Demo](#) · [Report Bug](#) · [Request Feature](#)

</div>


---

## 📋 Table of Contents

- [About the Project](#-about-the-project)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Architecture](#-architecture)
- [Getting Started](#-getting-started)
- [Environment Variables](#-environment-variables)
- [API Reference](#-api-reference)
- [Project Structure](#-project-structure)
- [Key Implementation Details](#-key-implementation-details)
- [Deployment](#-deployment)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🧠 About the Project

** full-stack blog** is a production-grade full-stack blog application that brings together the latest versions of React and Express to demonstrate real-world engineering patterns. It's not a tutorial toy — it's built like a product.

Users can **discover** posts with infinite scroll, **search and filter** by category, sort by trending or popularity, **write and publish** using a rich text editor with inline media uploads, and **engage** through an optimistic comment system. The platform supports **role-based access control** with admin capabilities like featuring and moderating content.

> Built to showcase: React Query data management, Express 5 async error handling, Clerk authentication with webhooks, ImageKit CDN media optimization, and full VPS deployment.

---

## ✨ Features

### 📖 Reading & Discovery
- ♾️ **Infinite scroll** pagination — posts load as you scroll, no page buttons
- 🔍 **Real-time search** — filter posts by title via URL search params
- 🏷️ **Category filtering** — browse by Web Design, Development, Databases, SEO, Marketing
- 📊 **Sort controls** — Newest, Oldest, Most Popular, Trending (last 7 days by visit count)
- 👤 **Author filtering** — view all posts by a specific author

### ✍️ Writing & Publishing
- 📝 **Rich text editor** (React Quill) — bold, italic, lists, links, headings
- 🖼️ **Cover image upload** with progress tracking
- 📎 **Inline image & video upload** directly inside post content
- 🔗 **Auto-generated unique slugs** from post title with collision handling
- 🗂️ **Category tagging** for every post

### 💬 Interaction
- 🗨️ **Comment system** with real-time cache revalidation
- ⚡ **Optimistic UI** — comments and saves appear instantly before server confirms
- 🔖 **Save / unsave posts** — bookmark posts to your profile
- 🗑️ **Delete** your own comments and posts

### 🔐 Authentication & Roles
- 🔑 **Clerk authentication** — email/password + Google OAuth
- 👑 **Admin role** — feature posts, delete any post or comment
- 🛡️ **JWT-protected routes** — server-side token verification on all mutations
- 🪝 **Webhook user sync** — Clerk users automatically synced to MongoDB

### 🚀 Performance
- 📦 **ImageKit CDN** — on-the-fly image resizing, up to 90% payload reduction
- 🌫️ **LQIP** (Low Quality Image Placeholder) — blurred previews while images load
- 🔄 **React Query caching** — smart cache invalidation, no redundant API calls
- 📱 **Fully responsive** — optimized for mobile, tablet, and desktop

---

## 🛠️ Tech Stack

### Frontend
| Technology | Purpose |
|---|---|
| **React 19** | UI framework |
| **Vite** | Build tool & dev server |
| **Tailwind CSS** | Utility-first styling |
| **React Router v6** | Client-side routing |
| **TanStack React Query** | Server state, caching, mutations |
| **Axios** | HTTP client |
| **React Quill New** | Rich text editor |
| **ImageKit React SDK** | Optimized image delivery & upload |
| **React Infinite Scroll** | Infinite pagination |
| **React Toastify** | Toast notifications |
| **Timeago.js** | Relative date formatting |
| **Clerk React** | Authentication UI & hooks |

### Backend
| Technology | Purpose |
|---|---|
| **Node.js** | Runtime |
| **Express 5** | Web framework (native async errors) |
| **MongoDB Atlas** | Cloud NoSQL database |
| **Mongoose** | ODM — schemas, models, validation |
| **Clerk Express** | JWT middleware & session claims |
| **Svix** | Webhook signature verification |
| **ImageKit Node** | Server-side upload authentication |
| **Body Parser** | Raw body for webhook verification |
| **CORS** | Cross-origin request handling |
| **dotenv** | Environment variable management |

### Infrastructure
| Technology | Purpose |
|---|---|
| **Hostinger VPS** | Cloud server hosting |
| **CloudPanel** | Server management UI |
| **PM2** | Process manager, auto-restart |
| **Let's Encrypt** | Free SSL certificates |
| **ngrok** | Local webhook tunneling (dev) |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                        CLIENT                           │
│  React 19 + Vite + Tailwind + React Query + Clerk       │
│                                                         │
│  ┌──────────┐  ┌──────────┐  ┌──────────────────────┐  │
│  │  Pages   │  │Components│  │   React Query Cache  │  │
│  │ Homepage │  │ Navbar   │  │  posts / comments /  │  │
│  │ PostList │  │ PostItem │  │  savedPosts / post   │  │
│  │ Single   │  │ Comments │  └──────────────────────┘  │
│  │ Write    │  │ Upload   │                             │
│  └──────────┘  └──────────┘                             │
└───────────────────────┬─────────────────────────────────┘
                        │ HTTPS + Bearer JWT
┌───────────────────────▼─────────────────────────────────┐
│                    EXPRESS 5 API                         │
│                                                         │
│  clerkMiddleware → cors → express.json → routes         │
│                                                         │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐   │
│  │  /posts  │ │/comments │ │  /users  │ │/webhooks │   │
│  └────┬─────┘ └────┬─────┘ └────┬─────┘ └────┬─────┘   │
│       │             │            │             │         │
│  ┌────▼─────────────▼────────────▼─────────────▼─────┐  │
│  │              Controllers + Models                  │  │
│  └────────────────────────┬───────────────────────────┘  │
└───────────────────────────┼─────────────────────────────┘
                            │
          ┌─────────────────┼──────────────────┐
          │                 │                  │
┌─────────▼──────┐ ┌────────▼──────┐ ┌────────▼───────┐
│  MongoDB Atlas  │ │  ImageKit CDN │ │  Clerk (Auth)  │
│  Users/Posts/   │ │  Media Store  │ │  Sessions/JWT  │
│  Comments       │ │  + Transform  │ │  + Webhooks    │
└─────────────────┘ └───────────────┘ └────────────────┘
```

---

## 🚀 Getting Started

### Prerequisites

- Node.js `v18+`
- npm `v8+`
- A [MongoDB Atlas](https://cloud.mongodb.com) account (free tier works)
- A [Clerk](https://clerk.com) account (free tier works)
- An [ImageKit](https://imagekit.io) account (free tier works)

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/blog.git
cd blog
```

### 2. Set up the Backend

```bash
cd backend
npm install
```

Create your `.env` file (see [Environment Variables](#-environment-variables) section):

```bash
cp .env.example .env
# Fill in your values
```

Start the development server:

```bash
node --env-file=.env --watch index.js
```

> The API will be running at `http://localhost:3000`

### 3. Set up the Frontend

```bash
cd ../client
npm install --force  # --force required for React 19 peer deps
```

Create your `.env` file:

```bash
cp .env.example .env
# Fill in your values
```

Start the development server:

```bash
npm run dev
```

> The app will be running at `http://localhost:5173`

### 4. Set up Webhooks (for user sync)

Install [ngrok](https://ngrok.com) and expose your local backend:

```bash
ngrok http 3000
```

Copy the HTTPS URL (e.g. `https://abc123.ngrok.io`) and add it to Clerk Dashboard:

```
Clerk Dashboard → Webhooks → Add Endpoint
URL: https://abc123.ngrok.io/webhooks/clerk
Events: user.created, user.updated, user.deleted
```

Copy the webhook secret and add it to `backend/.env` as `CLERK_WEBHOOK_SECRET`.

---

## 🔐 Environment Variables

### Backend — `backend/.env`

```env
# Server
PORT=3000
NODE_ENV=development
CLIENT_URL=http://localhost:5173

# Database
MONGO_URI=mongodb+srv://<username>:<password>@cluster.mongodb.net/blog

# Clerk
CLERK_PUBLISHABLE_KEY=pk_test_xxxxxxxxxxxxxxxxxx
CLERK_SECRET_KEY=sk_test_xxxxxxxxxxxxxxxxxx
CLERK_WEBHOOK_SECRET=whsec_xxxxxxxxxxxxxxxxxx

# ImageKit
IK_URL_ENDPOINT=https://ik.imagekit.io/your-id
IK_PUBLIC_KEY=public_xxxxxxxxxxxxxxxxxx
IK_PRIVATE_KEY=private_xxxxxxxxxxxxxxxxxx
```

### Frontend — `client/.env`

```env
# API
VITE_API_URL=http://localhost:3000

# Clerk
VITE_CLERK_PUBLISHABLE_KEY=pk_test_xxxxxxxxxxxxxxxxxx

# ImageKit (public keys only — never put private key here)
VITE_IK_URL_ENDPOINT=https://ik.imagekit.io/your-id
VITE_IK_PUBLIC_KEY=public_xxxxxxxxxxxxxxxxxx
```

> ⚠️ **Never commit `.env` files to Git.** Both are listed in `.gitignore` by default.

---

## 📡 API Reference

### Posts

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `GET` | `/posts` | ❌ | Get all posts (paginated, filterable) |
| `GET` | `/posts/:slug` | ❌ | Get single post + increment visit |
| `POST` | `/posts` | ✅ | Create a new post |
| `DELETE` | `/posts/:id` | ✅ | Delete post (owner or admin) |
| `PATCH` | `/posts/feature` | ✅ Admin | Toggle featured status |
| `GET` | `/posts/upload-auth` | ❌ | Get ImageKit upload auth params |

#### Query Parameters for `GET /posts`

| Param | Type | Description |
|---|---|---|
| `page` | number | Page number (default: 1) |
| `limit` | number | Posts per page (default: 10) |
| `cat` | string | Filter by category |
| `search` | string | Search in post titles |
| `author` | string | Filter by username |
| `sort` | `newest` \| `oldest` \| `popular` \| `trending` | Sort order |
| `featured` | boolean | Show only featured posts |

**Example:**
```
GET /posts?cat=development&sort=trending&limit=5&page=1
```

---

### Comments

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `GET` | `/comments/:postId` | ❌ | Get all comments for a post |
| `POST` | `/comments/:postId` | ✅ | Add a comment to a post |
| `DELETE` | `/comments/:id` | ✅ | Delete comment (owner or admin) |

---

### Users

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `GET` | `/users/saved` | ✅ | Get current user's saved post IDs |
| `PATCH` | `/users/save` | ✅ | Toggle save/unsave a post |

---

### Webhooks

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/webhooks/clerk` | Receives Clerk user lifecycle events |

---

## 📁 Project Structure

```
blog/
│
├── client/                          # React Frontend
│   ├── public/                      # Static assets
│   ├── src/
│   │   ├── components/
│   │   │   ├── Comment.jsx          # Single comment with delete
│   │   │   ├── Comments.jsx         # Comment list + add form
│   │   │   ├── FeaturedPosts.jsx    # Homepage featured section
│   │   │   ├── Image.jsx            # ImageKit optimized image
│   │   │   ├── MainCategories.jsx   # Category nav + search bar
│   │   │   ├── Navbar.jsx           # Responsive navigation
│   │   │   ├── PostList.jsx         # Infinite scroll post list
│   │   │   ├── PostListItem.jsx     # Single post list card
│   │   │   ├── PostMenuActions.jsx  # Save / Delete / Feature actions
│   │   │   ├── Search.jsx           # Search input with URL sync
│   │   │   ├── SideMenu.jsx         # Filters, categories, search
│   │   │   └── Upload.jsx           # ImageKit file upload wrapper
│   │   │
│   │   ├── layouts/
│   │   │   └── MainLayout.jsx       # Persistent Navbar + Outlet
│   │   │
│   │   ├── routes/
│   │   │   ├── Homepage.jsx         # Landing page
│   │   │   ├── LoginPage.jsx        # Clerk SignIn component
│   │   │   ├── PostListPage.jsx     # Post listing with filters
│   │   │   ├── RegisterPage.jsx     # Clerk SignUp component
│   │   │   ├── SinglePostPage.jsx   # Full post + comments
│   │   │   └── Write.jsx            # Create / edit post form
│   │   │
│   │   ├── index.css                # Global styles
│   │   └── main.jsx                 # App entry, providers setup
│   │
│   ├── .env                         # Frontend env vars (gitignored)
│   ├── .env.example                 # Template for env vars
│   └── package.json
│
├── backend/                         # Express API
│   ├── controllers/
│   │   ├── comment.controller.js    # Comment CRUD logic
│   │   ├── post.controller.js       # Post CRUD + filter + upload auth
│   │   ├── user.controller.js       # Saved posts management
│   │   └── webhook.controller.js    # Clerk event processing
│   │
│   ├── lib/
│   │   └── connectDB.js             # MongoDB connection
│   │
│   ├── middlewares/
│   │   └── increaseVisit.js         # Visit counter middleware
│   │
│   ├── models/
│   │   ├── comment.model.js         # Comment schema
│   │   ├── post.model.js            # Post schema
│   │   └── user.model.js            # User schema
│   │
│   ├── routes/
│   │   ├── comment.route.js         # Comment endpoints
│   │   ├── post.route.js            # Post endpoints
│   │   ├── user.route.js            # User endpoints
│   │   └── webhook.route.js         # Webhook endpoint
│   │
│   ├── index.js                     # Express app entry
│   ├── .env                         # Backend env vars (gitignored)
│   ├── .env.example                 # Template for env vars
│   └── package.json
│
└── README.md
```

---

## 🔍 Key Implementation Details

### ♾️ Infinite Scroll with Dynamic Filters

Infinite scroll and URL-based filters work together by including `searchParams.toString()` in the React Query key. When filters change, the key changes, resetting pagination to page 1 and fetching fresh filtered data.

```js
const { data, fetchNextPage, hasNextPage } = useInfiniteQuery({
  queryKey: ["posts", searchParams.toString()], // key changes = cache reset
  queryFn: ({ pageParam = 1 }) => fetchPosts({ pageParam, ...filters }),
  getNextPageParam: (lastPage) => lastPage.hasMore ? lastPage.currentPage + 1 : undefined,
});
```

---

### ⚡ Optimistic UI for Comments

Comments appear instantly on send using `mutation.variables` — the data passed to `mutate()` — before the server confirms.

```jsx
{mutation.isPending && (
  <Comment comment={{
    desc: mutation.variables?.desc,
    createdAt: new Date(),
    user: { username: user.username, img: user.imageUrl }
  }} />
)}
```

After `invalidateQueries` fires on success, the list refreshes with real server data and the optimistic comment is replaced automatically.

---

### 🛡️ Express 5 Global Error Handling

No try-catch blocks in controllers. Express 5 catches async errors automatically and passes them to the global error handler.

```js
// Controller — no try/catch needed
export const getPosts = async (req, res) => {
  const posts = await Post.find(query); // throws? Express 5 handles it
  res.json(posts);
};

// Global handler — one place for all errors
app.use((error, req, res, next) => {
  res.status(error.status || 500).json({
    message: error.message || "Something went wrong",
    stack: process.env.NODE_ENV === "production" ? null : error.stack,
  });
});
```

---

### 🖼️ ImageKit On-the-Fly Optimization

Images are resized at the CDN level to exactly the width they'll be displayed at — no more serving 3MB images in 300px slots.

```jsx
// Serving a 298px-wide image for a post list thumbnail
<Image src={post.img} w={298} className="rounded-2xl object-cover" />

// ImageKit transformation: ?tr=w-298 applied automatically
```

LQIP (Low Quality Image Placeholder) shows a blurred preview while the full image loads, eliminating layout shifts.

---

### 🔄 Visit Counter Middleware

Express middleware increments the visit count atomically before the post is served — no separate API call needed from the frontend.

```js
export const increaseVisit = async (req, res, next) => {
  await Post.findOneAndUpdate(
    { slug: req.params.slug },
    { $inc: { visit: 1 } }  // atomic increment
  );
  next(); // pass to getPost controller
};

router.get("/:slug", increaseVisit, getPost); // middleware chain
```

---

### 👑 Admin Role via Clerk Session Token

The admin role is stored in Clerk's public metadata and embedded in the session JWT — available on both frontend and backend without an extra database call.

```js
// Backend: Read from JWT claims
const role = req.auth?.sessionClaims?.metadata?.role;
if (role === "admin") { /* bypass ownership check */ }
```

```jsx
// Frontend: Read from Clerk user object
const isAdmin = user?.publicMetadata?.role === "admin";
```

---

## 🚢 Deployment

### Deploy on a VPS (Hostinger / DigitalOcean / Linode)

#### 1. Set Up Server

Purchase a VPS and install **CloudPanel** as the control panel. Create two Node.js applications:
- `blog-api.yourdomain.com` → port `3000`
- `blog.yourdomain.com` → port `5173`

Install SSL certificates for both domains via CloudPanel.

#### 2. SSH into Server & Deploy Backend

```bash
# Connect
ssh root@your-server-ip

# Switch to app user
sudo su - your-backend-user
cd htdocs/blog-api.yourdomain.com

# Clone & install
git clone https://github.com/yourusername/blog.git .
npm install

# Create env file
nano .env  # paste all backend env vars

# Install PM2 & start
npm install -g pm2
pm2 start index.js --name api --watch
pm2 save
```

#### 3. Deploy Frontend

```bash
sudo su - your-frontend-user
cd htdocs/blog.yourdomain.com

git clone https://github.com/yourusername/blog.git .
npm install --force
nano .env  # paste all frontend env vars (use production HTTPS URLs)

# Build & serve
NODE_ENV=production npm run build
pm2 serve dist 5173 --name client --spa
pm2 save
```

#### 4. Post-Deployment Checklist

- [ ] Update `CLIENT_URL` in backend `.env` to production domain
- [ ] Update `VITE_API_URL` in frontend `.env` to production API domain
- [ ] Update Clerk webhook URL from ngrok to production URL
- [ ] Restrict MongoDB Atlas Network Access to server IP only
- [ ] Verify SSL certificates are active on both domains
- [ ] Test auth flow, post creation, image upload end-to-end

---

## 🧩 Contributing

Contributions are welcome! Here's how to get started:

1. **Fork** the repository
2. **Create** your feature branch
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. **Commit** your changes
   ```bash
   git commit -m "feat: add amazing feature"
   ```
4. **Push** to the branch
   ```bash
   git push origin feature/amazing-feature
   ```
5. **Open** a Pull Request

### Commit Convention

This project uses [Conventional Commits](https://www.conventionalcommits.org):

| Prefix | Use for |
|---|---|
| `feat:` | A new feature |
| `fix:` | A bug fix |
| `docs:` | Documentation changes |
| `style:` | Formatting, missing semicolons, etc. |
| `refactor:` | Code change that's neither a fix nor feature |
| `perf:` | Performance improvements |
| `test:` | Adding missing tests |

---

## 🗺️ Roadmap

- [ ] TypeScript migration
- [ ] Real-time comments via WebSockets
- [ ] Email notifications for new comments
- [ ] Draft / schedule post publishing
- [ ] Post analytics dashboard for authors
- [ ] Dark mode
- [ ] SEO improvements (meta tags, Open Graph)
- [ ] Search with debouncing
- [ ] User profile pages
- [ ] Post reaction system (likes/claps)

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.

---

## 🙏 Acknowledgements

- [Clerk](https://clerk.com) — authentication that just works
- [ImageKit](https://imagekit.io) — blazing fast media delivery
- [TanStack Query](https://tanstack.com/query) — the best thing to happen to React data fetching
- [Tailwind CSS](https://tailwindcss.com) — utility-first CSS done right
- [MongoDB Atlas](https://cloud.mongodb.com) — generous free tier for hobby projects

---

<div align="center">

Made with ❤️ and too much ☕

⭐ **Star this repo if you found it useful!** ⭐

</div>
