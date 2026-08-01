
  # WEB project from 262

# 🐾 PawSphere — Pet Adoption & Veterinary Care Platform

PawSphere is a modern, user-friendly platform designed to simplify pet adoption and veterinary care in Bangladesh. It connects pet owners, animal shelters, veterinarians, and platform administrators through a unified and organized experience.

This project is currently focused on delivering a polished front-end prototype featuring role-based dashboards, realistic mock data, interactive UI states, adoption workflows, and veterinary appointment management.

---

## ✨ Features

### 🌐 Public Experience

- Landing page introducing PawSphere
- Browse and search pets
- Filter pets by species (dogs and cats)
- Detailed pet profiles with:
  - Image gallery carousel
  - Personality tags
  - Lifestyle tags
  - Health notes
  - Vaccination status
  - Shelter information
  - Verified shelter badges
- Save or favorite pets
- Sign in and registration screens

---

### 👤 Pet Owner Portal

- Dashboard overview
- Quick action cards
- Upcoming appointment reminders
- Browse pets and manage favorites
- Track adoption applications
- View pet health and vaccination records
- Appointment history and cancellation flow
- Search veterinarians and book appointments
- Messages and notifications
- Downloadable pet documents, certificates, and adoption agreements

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
- Manage pet listings
- Add new pets
- Review adoption applications
- Schedule meet-and-greets
- Generate adoption certificates
- View reports and analytics with charts
- Quick actions and status tracking

---

### 🩺 Veterinarian Portal

- Veterinarian dashboard
- Appointment management
- Patient record management
- Prescriptions
- Medical history
- Messaging
- Verified veterinarian indicators

---

### 🛡️ Administrator Portal

- Platform dashboard
- User management
- Shelter and veterinarian verification workflows
- Adoption monitoring
- Pet listing moderation
- Broadcast management
- Feedback and system logs
- Platform analytics and reporting

---

## 🎨 UX & UI Enhancements

- First-login welcome modal for each role
- Success toast notifications for important actions
- Loading skeletons for pet browsing
- Subtle hover, shadow, and elevation animations
- Helpful empty states with next-step guidance
- Verified badges for shelters and veterinarians
- Responsive layouts for smaller screens
- Unique chart IDs to avoid Recharts duplicate-key warnings

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

<Code value="text"/><CodeBlock language="text" editable={false} interactive={false} content="PawSphere/
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
"/>

> **Note:** Most application screens and logic are intentionally consolidated inside `src/app/App.tsx` for this prototype version.

---

## 🏗️ System Architecture

PawSphere currently uses a **front-end-only role-based architecture**.

<Code value="text"/><CodeBlock language="text" editable={false} interactive={false} content="User
├── Guest / Public Visitor
│   ├── Landing page
│   ├── Browse pets
│   ├── Pet details
│   └── Authentication
│
└── Authenticated User
    ├── Pet Owner Portal
    ├── Shelter Portal
    ├── Veterinarian Portal
    └── Administrator Portal
"/>

---

## 🔄 Current Application Flow

<List gap={2}><List.Item>Visitor lands on the public homepage.</List.Item><List.Item>Browses pets and opens pet detail pages.</List.Item><List.Item>Uses the login screen to access a demo role.</List.Item><List.Item>Receives a welcome modal after login.</List.Item><List.Item>Enters a role-specific dashboard and navigation experience.</List.Item><List.Item>All screens use local mock data and local UI state updates.</List.Item></List>

---

## 📋 Prerequisites

Install the following before running the project locally:

- **Node.js 18+**
- **pnpm**

Check installed versions:

<Code value="bash"/><CodeBlock language="bash" editable={true} interactive={true} content="node --version
pnpm --version
"/>

Install pnpm globally if needed:

<Code value="bash"/><CodeBlock language="bash" editable={true} interactive={true} content="npm install -g pnpm
"/>

---

## ⚙️ Installation

Clone or download the project, then install dependencies:

<Code value="bash"/><CodeBlock language="bash" editable={true} interactive={true} content="pnpm install
"/>

---

## ▶️ Run Locally

Start the development server:

<Code value="bash"/><CodeBlock language="bash" editable={true} interactive={true} content="pnpm dev
"/>

If using the direct Vite command:

<Code value="bash"/><CodeBlock language="bash" editable={true} interactive={true} content="pnpm vite
"/>

Open the local URL shown in the terminal (commonly):

<Code value="text"/><CodeBlock language="text" editable={false} interactive={false} content="http://localhost:5173
"/>

---

## 📦 Build for Production

Create a production build:

<Code value="bash"/><CodeBlock language="bash" editable={true} interactive={true} content="pnpm build
"/>

> **Note:** This workspace is currently configured for **Figma Make** and uses `__figma__entrypoint__.ts`. A standard standalone Vite deployment may require an `index.html` entry file.

---

## 📚 Main Dependencies

- react
- react-dom
- typescript
- vite
- tailwindcss
- lucide-react
- recharts
- motion
- sonner
- date-fns
- @radix-ui/*
- embla-carousel-react

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

<Code value="text"/><CodeBlock language="text" editable={false} interactive={false} content="Primary Green:      #16A34A
Dark Green:         #15803D
Light Green:        #DCFCE7
Soft Green Surface: #F0FDF4
Page Background:    #F8FAFC
Primary Text:       #111827
Secondary Text:     #6B7280
Border:             #E5E7EB
"/>

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

## 🚀 Suggested Future Improvements

### Backend & Authentication

- Secure authentication for each role
- Database integration
- Role-based access control
- Persistent pet listings, applications, and appointments

### Pet Adoption Workflow

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

- Pet Owner
- Shelter
- Veterinarian
- Administrator

Each role opens a different dashboard and navigation experience.

---

## 🙏 Credits

- **Pet & veterinarian images:** Unsplash
- **Icons:** Lucide React
- **Charts:** Recharts
- **UI Framework:** React + Tailwind CSS

---

## 📌 Status

PawSphere is currently in the **front-end prototype stage**. The project demonstrates the complete user experience, role-based navigation, and core adoption and veterinary workflows, while backend services and real-world integrations are planned for future development.
  
