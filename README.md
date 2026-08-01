
  # WEB project from 262

# 🐾 PawSphere

### Pet Adoption & Veterinary Care Platform

PawSphere is a modern, user-friendly platform designed to simplify **pet adoption and veterinary care in Bangladesh**. It brings together **pet owners, animal shelters, veterinarians, and platform administrators** into one organized digital experience.

The current version focuses on delivering a **polished front-end prototype** with role-based dashboards, realistic mock data, interactive UI states, adoption workflows, and veterinary appointment management.

---

## ✨ Key Features

### 🌐 Public Experience

- Landing page introducing PawSphere
- Browse and search pets
- Filter pets by species (dogs and cats)
- Detailed pet profile pages
- Favorite / save pet interactions
- Public sign-in and registration screens

#### Pet Profile Includes

- Image gallery carousel
- Personality tags
- Lifestyle tags
- Health notes
- Vaccination status
- Shelter information
- Verified shelter badges

---

### 👤 Pet Owner Portal

- Dashboard overview
- Quick action cards
- Upcoming appointment reminders
- Browse pets and manage favorites
- Track adoption applications
- Pet health and vaccination records
- Appointment history and cancellation flow
- Search veterinarians and book appointments
- Messages and notifications
- Downloadable pet documents and adoption agreements

#### Friendly Empty States

- No pets found
- No saved pets
- No saved veterinarians
- No appointments
- No messages
- No applications in the selected status

---

### 🏠 Shelter Portal

- Shelter dashboard
- Pet listing management
- Add-pet workflow
- Adoption application review
- Meet-and-greet scheduling
- Adoption certificates
- Reports and analytics
- Quick actions and status tracking

---

### 🩺 Veterinarian Portal

- Veterinarian dashboard
- Appointment management
- Patient records
- Prescriptions
- Medical history
- Messaging
- Verified veterinarian indicators

---

### 🛡️ Administrator Portal

- Platform dashboard
- User management
- Shelter and veterinarian verification
- Adoption monitoring
- Pet listing moderation
- Broadcast management
- Feedback and system logs
- Platform analytics and reporting

---

## 🎨 UX & UI Enhancements

- First-login welcome modal for each role
- Success toast notifications
- Loading skeletons for pet browsing
- Subtle hover, shadow, and elevation animations
- Clean empty states with helpful guidance
- Verified badges for shelters and veterinarians
- Responsive layouts for smaller screens
- Unique chart IDs to prevent Recharts duplicate-key warnings

---

## 🛠️ Technology Stack

### Frontend

- React 18
- TypeScript
- Vite
- Tailwind CSS

### UI & Visual Libraries

- Lucide React
- Recharts
- Motion
- Sonner
- Radix UI
- Embla Carousel

### Styling

- Tailwind CSS utility classes
- Shared design tokens in `src/styles/theme.css`
- Google Font: **Manrope**
- Primary accent color: **#16A34A**

### Data

- Mock data only (no backend integration yet)

---

## 📁 Project Structure

```text
PawSphere/
├── src/
│   ├── app/
│   │   └── App.tsx
│   ├── styles/
│   │   ├── fonts.css
│   │   ├── theme.css
│   │   ├── index.css
│   │   └── globals.css
│   └── imports/
│       └── PawSphere_Project_Description.pdf
│
├── package.json
├── vite.config.ts
├── postcss.config.mjs
└── __figma__entrypoint__.ts
```

> **Note:** Most application screens and prototype logic are intentionally consolidated inside `src/app/App.tsx`.

---

## 🏗️ System Architecture

PawSphere currently follows a **front-end-only role-based architecture**.

```text
User
├── Guest / Public Visitor
│   ├── Landing Page
│   ├── Browse Pets
│   ├── Pet Details
│   └── Authentication
│
└── Authenticated User
    ├── Pet Owner Portal
    ├── Shelter Portal
    ├── Veterinarian Portal
    └── Administrator Portal
```

---

## 🔄 Current Application Flow

1. Visitor lands on the public homepage.
2. Browses pets and opens pet detail pages.
3. Selects a demo role from the login screen.
4. Receives a welcome modal after login.
5. Enters a role-specific dashboard and navigation experience.
6. All screens use local mock data and local UI state updates.

---

## 🚀 Getting Started

### Prerequisites

Install the following before running the project locally:

- Node.js 18 or later
- pnpm

Check installed versions:

```bash
node --version
pnpm --version
```

Install pnpm globally if needed:

```bash
npm install -g pnpm
```

---

### Installation

Clone or download the project, then install dependencies:

```bash
pnpm install
```

---

### Run Locally

Start the development server:

```bash
pnpm dev
```

Open:

```text
http://localhost:5173
```

---

### Build for Production

```bash
pnpm build
```

> **Note:** This workspace is currently configured for **Figma Make** and uses `__figma__entrypoint__.ts`. A standard standalone Vite deployment may require an `index.html` entry file.

---

## 🎯 Design System

PawSphere follows a **minimal, trustworthy health-and-care SaaS design language**.

### Visual Principles

- Clean white card surfaces
- Spacious layouts
- Clear hierarchy
- Calm green accents
- Subtle borders and soft shadows
- Minimal decoration
- Accessible contrast
- Rounded cards and form controls

### Core Colors

```text
Primary Green:      #16A34A
Dark Green:         #15803D
Light Green:        #DCFCE7
Soft Green Surface: #F0FDF4
Page Background:    #F8FAFC
Primary Text:       #111827
Secondary Text:     #6B7280
Border:             #E5E7EB
```

---

## ⚠️ Current Limitations

This version is a **front-end prototype** and does not yet include:

- Real authentication
- Database storage
- API integration
- Real appointment booking
- Real messaging
- File uploads
- PDF generation
- Email notifications
- Payment processing
- Maps or location services
- Real-time updates
- Advanced analytics

All users, pets, appointments, messages, records, documents, and reports are currently represented using **mock data**.

---

## 🚧 Future Improvements

### Backend & Authentication

- Secure authentication for each role
- Database integration
- Role-based access control
- Persistent pet listings and appointments

### Adoption Workflow

- Real application forms
- Status updates
- Shelter review tools
- Meet-and-greet scheduling
- Adoption agreement management

### Veterinary Care

- Real appointment booking
- Digital prescriptions
- Medical record uploads
- Vaccination reminders
- Downloadable care documents

### Notifications

- Email notifications
- Persistent in-app notifications
- Appointment reminders
- Adoption updates

### Admin Tools

- Real verification workflows
- Listing approvals
- Content moderation
- System activity logs

---

## 🧪 Demo Roles

The sign-in screen currently supports demo access for:

```text
Pet Owner
Shelter
Veterinarian
Administrator
```

Each role opens a different dashboard and navigation experience.

---

## 📊 Project Status

> **Status:** Front-end prototype in active development

The current build demonstrates the **complete user experience, role-based navigation, adoption workflow, and veterinary care flow** using realistic mock data and interactive UI behavior.

---

## 🙏 Credits

- **Pet & veterinarian images:** Unsplash
- **Icons:** Lucide React
- **Charts:** Recharts
- **UI Framework:** React + Tailwind CSS

---

## 📄 License

This project is currently intended for **educational, prototype, and portfolio purposes**.
