# DisputeX

**A Full-Stack Dispute Management Platform for Merchants and Administrators**

DisputeX is a web-based dispute management platform designed to simplify payment dispute handling, evidence submission, and case tracking. It provides merchants with tools to manage disputes and submit supporting documents, while administrators get a centralized dashboard to monitor dispute activity, merchants, notifications, and escalations.

<p align="center">
  <a href="https://s116-0726-brainrot-full-stack-with.vercel.app/"><strong>Live Application</strong></a>
  ·
  <a href="https://github.com/Yashraj191007/S116-0726-Brainrot-Full-Stack-with-Nextjs-and-Cloudinary-DisputeX"><strong>GitHub Repository</strong></a>
</p>

---

## Overview

Managing payment disputes requires merchants to collect evidence, monitor deadlines, and track case progress. DisputeX brings these workflows together in a single platform.

The application provides separate experiences for merchants and administrators, supported by a REST API, PostgreSQL database, and cloud-based file storage.

## Key Features

- **Role-Based Access:** Separate merchant and administrator experiences.
- **Authentication:** Secure registration and login using JWT-based authentication.
- **Dispute Management:** Create, view, and track disputes throughout their lifecycle.
- **Evidence Upload:** Upload supporting PDF and image files using Cloudinary storage.
- **Activity Tracking:** Maintain a timeline of important dispute actions and status changes.
- **Admin Dashboard:** Monitor dispute statistics, merchant counts, recent cases, and activity.
- **Notifications:** View notifications and manage their read status.
- **Automated Workflows:** Scheduled reminder and escalation jobs for dispute deadlines.
- **Input Validation:** Validate incoming API requests and handle errors centrally.

## Technology Stack

| Layer | Technologies |
|---|---|
| Frontend | Next.js 16, React 19, TypeScript |
| Styling | Tailwind CSS 4, shadcn/ui |
| UI Components | Lucide React |
| Data Visualization | Recharts |
| Backend | Node.js, Express.js, TypeScript |
| Database | PostgreSQL |
| ORM | Prisma |
| Authentication | JWT, bcryptjs |
| File Storage | Cloudinary |
| Background Jobs | node-cron |
| API Communication | Axios |
| Frontend Deployment | Vercel |

## Project Architecture

The repository follows a modular full-stack architecture with separate frontend and backend applications.

```text
DisputeX/
│
├── frontend/
│   ├── app/
│   │   ├── admin/           # Admin dashboard and management
│   │   ├── dashboard/       # Merchant dashboard
│   │   ├── disputes/        # Dispute listing and details
│   │   ├── evidence/        # Evidence upload and history
│   │   ├── notifications/   # Notifications
│   │   ├── profile/         # User profile
│   │   └── settings/        # Application settings
│   │
│   ├── components/          # Reusable UI components
│   ├── features/            # Feature-specific modules
│   ├── hooks/               # Custom React hooks
│   ├── services/            # API service functions
│   ├── lib/                 # Shared utilities and API client
│   └── types/               # TypeScript definitions
│
├── backend/
│   ├── config/              # Application configuration
│   ├── controllers/         # Request handlers
│   ├── jobs/                # Reminder and escalation jobs
│   ├── middleware/          # Authentication and validation
│   ├── prisma/              # Database schema and configuration
│   ├── repositories/        # Database access layer
│   ├── routes/              # REST API routes
│   ├── services/            # Business logic
│   ├── storage/             # File storage integration
│   ├── validators/          # Request validation
│   └── server.ts            # Backend entry point
│
├── database/                # Database schemas and scripts
├── docs/                    # Technical documentation
├── tests/                   # Test scaffolding
├── package.json
└── README.md
```

## Core Functionalities

### 1. Authentication and Authorization

- Merchant and administrator registration and login.
- Password hashing using bcryptjs.
- JWT-based authentication.
- Protected API routes using authentication middleware.

### 2. Dispute Management

Disputes can be created, retrieved, and updated through the backend API.

Each dispute maintains information such as:

- Dispute amount and currency.
- Dispute reason.
- Merchant association.
- Current dispute status.
- Supporting evidence URL.
- Creation and update timestamps.

### 3. Evidence Management

- Upload supporting documents and images.
- Validate file types and file sizes.
- Store uploaded evidence through Cloudinary.
- Associate evidence with the relevant dispute.
- Maintain activity records when evidence is submitted.

### 4. Dispute Lifecycle

DisputeX supports the following dispute statuses:

| Status | Description |
|---|---|
| OPEN | Newly created dispute awaiting action. |
| UNDER_REVIEW | Evidence has been submitted for review. |
| ESCALATED | Dispute has passed its response deadline. |
| WON | Dispute resolved in the merchant's favour. |
| LOST | Dispute resolved against the merchant. |

### 5. Admin Dashboard

Administrators can access an overview of:

- Total disputes.
- Open and escalated disputes.
- Won and lost disputes.
- Total registered merchants.
- Recent disputes.
- Recent activity.
- Dispute activity statistics.

### 6. Notifications and Automated Jobs

The backend includes scheduled jobs using node-cron.

- **Reminder Job:** Generates reminders for open disputes approaching their response deadline.
- **Escalation Job:** Escalates eligible open disputes after the seven-day response period.
- **Activity Logging:** Records important dispute workflow events.

---

## Getting Started

Follow these steps to run DisputeX locally.

### Prerequisites

Make sure you have installed:

- Node.js (LTS recommended)
- npm
- PostgreSQL
- Cloudinary account

### 1. Clone the Repository

```bash
git clone https://github.com/Yashraj191007/S116-0726-Brainrot-Full-Stack-with-Nextjs-and-Cloudinary-DisputeX.git

cd S116-0726-Brainrot-Full-Stack-with-Nextjs-and-Cloudinary-DisputeX
```

### 2. Install Dependencies

From the project root:

```bash
npm install
```

### 3. Configure Environment Variables

Create a `.env` file inside the `backend/` directory.

```env
NODE_ENV=development
PORT=8000

DATABASE_URL="your_postgresql_connection_string"

JWT_SECRET="your_secure_jwt_secret"
JWT_EXPIRES_IN=1d

FRONTEND_URL=http://localhost:3000

CLOUDINARY_CLOUD_NAME="your_cloud_name"
CLOUDINARY_API_KEY="your_api_key"
CLOUDINARY_API_SECRET="your_api_secret"
```

Create a `.env.local` file inside the `frontend/` directory.

```env
NEXT_PUBLIC_API_URL=http://localhost:8000/api
```

**Important:** Replace the placeholder values with your actual credentials. Never commit environment files, database credentials, API keys, or secrets to GitHub.

### 4. Configure the Database

Navigate to the backend directory:

```bash
cd backend
```

Generate the Prisma Client:

```bash
npx prisma generate
```

Synchronize the Prisma schema with your PostgreSQL database:

```bash
npx prisma db push
```

### 5. Run the Backend

From the `backend/` directory:

```bash
npm run dev
```

The backend runs at:

```text
http://localhost:8000
```

### 6. Run the Frontend

Open another terminal and navigate to the frontend directory:

```bash
cd frontend
npm run dev
```

Open the application:

```text
http://localhost:3000
```

---

## API Overview

The backend exposes RESTful API endpoints under the `/api` prefix.

| Module | Endpoint | Description |
|---|---|---|
| Authentication | `/api/auth` | Registration, login, and user details |
| Merchants | `/api/merchants` | Merchant operations |
| Disputes | `/api/disputes` | Dispute creation and management |
| Evidence | `/api/evidence` | Evidence upload and retrieval |
| Notifications | `/api/notifications` | Notification management |
| Administration | `/api/admin` | Administrative operations and statistics |

### Health Check

The backend also provides a health-check endpoint:

```http
GET /health
```

---

## Deployment

The frontend is deployed using Vercel.

**Live Application:**  
https://s116-0726-brainrot-full-stack-with.vercel.app/

For a complete deployment, the backend must also be hosted as an API service and connected to a PostgreSQL database and Cloudinary account.

Configure the deployed backend URL in the frontend environment:

```env
NEXT_PUBLIC_API_URL=your_deployed_backend_api_url
```

Configure the required database, authentication, and Cloudinary environment variables in the backend hosting provider.

---

## Documentation

Additional technical documentation is available in the `docs/` directory.

- [Frontend Documentation](docs/frontend.md)
- [Backend Documentation](docs/backend.md)
- [API Documentation](docs/api.md)
- [Architecture Documentation](docs/architecture.md)
- [Database Documentation](docs/database.md)
- [Development Guide](docs/development-guide.md)

---

## Testing

The repository includes initial test scaffolding under the `tests/` directory.

Automated test coverage and a configured backend test runner are areas for further development.

## Future Improvements

- Advanced dispute analytics and reporting.
- More granular role-based permissions.
- Comprehensive automated testing.
- Improved audit and compliance reporting.
- Enhanced notification delivery.
- Expanded dispute workflow automation.

---

<p align="center">
  <strong>DisputeX</strong><br/>
  Simplifying dispute management through technology.
</p>
