# 🚀 Next.js Project Specification and Configuration

Hi Claude! I need your help creating a complete, production-ready Next.js project. Please help me set up the following:

## 🛠️ Prject Configuration

- Next.js 15
- React 19
- Use TypeScript v5
- Tailwind CSS v4
- shadcn/ui library (compatible with Tailwind CSS v4)
- ESLint v9
- Use App Router
- Use Turbopack for `next dev`

## 📁 Required File Structure

```
src/
├── app/
│   ├── globals.css        # Tailwind imports
│   ├── layout.tsx         # RootLayout with metadata
│   └── page.tsx           # Main page component
├── components/
│   ├── ui/               # shadcn components
│   └── ClientWrapper.tsx # Client-side wrapper
├── lib/
    ├── constants.ts      # Global constants
    ├── types.ts          # TypeScript types
    └── utils.ts          # Utility functions
public/
```

## ⚙️ Configuration Files Needed

- `next.config.ts` (with strictMode and export configuration)
- `tailwind.config.ts` (with shadcn/ui setup)
- `package.json` (with all dependencies)
- `tsconfig.json`
- `.gitignore`

## 📦 Core Dependencies

```json
{
  "dependencies": {
    "next": "15.3.5",
    "react": "^19.0.0",
    "react-dom": "^19.0.0",
    
    "@radix-ui/react-select": "latest",
    "@radix-ui/react-slider": "latest",
    "@radix-ui/react-switch": "latest",
    
    "class-variance-authority": "latest",
    "clsx": "latest",
    "tailwind-merge": "latest",
    
    "lucide-react": "^0.525.0"
  },
  "devDependencies": {
    "typescript": "^5",
    "@types/node": "^20",
    "@types/react": "^19",
    "@types/react-dom": "^19",
    
    "eslint": "^9.18.0",
    "eslint-config-next": "^15.3.5",
    "@eslint/eslintrc": "^3",
    "@typescript-eslint/eslint-plugin": "^8.36.0",
    "@typescript-eslint/parser": "^8.36.0",
    
    "postcss": "^8.5.6",
    "tailwindcss": "^4.0.0-alpha.42",
    "@tailwindcss/postcss": "^4.0.0-alpha.42",
    "autoprefixer": "^10.4.21"
  }
}
```

## 🎨 Features to Include

- Responsive design
- Type-safe components
- Proper ESLint configuration
- Production-ready setup

## 🔧 Required Base Components

Please provide complete code for:

1. Basic Layout Component with:
   - Metadata configuration
   - Proper font setup (Inter)
   - Tailwind globals import

2. Utility Functions:
   - cn() utility for Tailwind class merging
   - Type-safe props handling
   - Common helper functions

3. shadcn/ui Components:
   - Basic Button
   - Card
   - Select
   - Switch

## 📝 Additional Requirements

- All components should be fully typed with TypeScript
- Include proper error boundaries
- Setup proper metadata in layout
- Include basic SEO configuration
- Ensure all components are accessible

## 🚦 Getting Started Instructions

Please provide the complete setup instructions including:

1. Installation commands
2. Development workflow
3. Building for production
4. Deployment considerations

I need this to be immediately runnable with minimal setup required. All code should be production-ready and follow best practices.

## Final Instructions

💡 After you acknowledge this request, I'll ask for the files one by one to ensure completeness.