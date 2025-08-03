# JDN DP LogBook - Project Configuration

> **Central configuration repository for the JDN DP LogBook maritime operations management system**

This repository contains shared configuration files and project documentation for the JDN DP LogBook web application - a comprehensive system for managing Dynamic Positioning (DP) logbooks, crew training records, and maritime operations.

## 📁 Repository Contents

### Configuration Files
- **`.claude/`** - Claude Code AI assistant settings and preferences
- **`.vscode/`** - Visual Studio Code workspace configuration
- **`claude.project.json`** - Claude project settings and context
- **`copilot-chat.json`** - GitHub Copilot configuration
- **`CLAUDE.md`** - Comprehensive project documentation and session history

## 🏗️ Project Architecture

The main application consists of two separate repositories:

### 🔧 Backend Repository: [dp-backend](https://github.com/flashroyal88/dp-backend)
- **Technology**: Node.js + TypeScript + Prisma ORM
- **Database**: MySQL with comprehensive maritime data models
- **Features**: REST API, user authentication, DP operations management
- **Key Models**: Profiles, Vessels, Training Types, Logbooks, DP Operations

### 🎨 Frontend Repository: [dp-web](https://github.com/flashroyal88/dp-web)  
- **Technology**: Next.js 14+ + TypeScript + Tailwind CSS
- **UI Components**: shadcn/ui component library
- **Features**: Responsive web interface, form validation, training management
- **Key Pages**: Login, Profile Management, Analytics, Crew Management, Vessel Operations

## 🚀 Quick Start

### 1. Clone Configuration (This Repository)
```bash
git clone https://github.com/flashroyal88/root.git
cd root
```

### 2. Clone Application Repositories
```bash
# Backend
git clone https://github.com/flashroyal88/dp-backend.git

# Frontend  
git clone https://github.com/flashroyal88/dp-web.git
```

### 3. Apply Configuration Files
Copy the configuration files from this repository to your development environment:

```bash
# Copy Claude Code settings
cp -r .claude/ /path/to/your/workspace/
cp claude.project.json /path/to/your/workspace/

# Copy VS Code settings
cp -r .vscode/ /path/to/your/workspace/

# Copy GitHub Copilot configuration
cp copilot-chat.json /path/to/your/workspace/
```

### 4. Setup Development Environment
```bash
# Backend setup
cd dp-backend
npm install
npx prisma generate
npm run dev  # Runs on localhost:4000

# Frontend setup (new terminal)
cd dp-web
npm install  
npm run dev  # Runs on localhost:3002
```

## 📋 Development Workflow

### Using Claude Code AI Assistant
This project is optimized for development with Claude Code. The configuration provides:

- **Project Context**: Automatic loading of project documentation
- **Session Continuity**: Persistent conversation history and task tracking
- **Code Quality Standards**: Established patterns and best practices
- **Troubleshooting Guide**: Common issues and solutions documented

### Key Development Commands
```bash
# Backend
npm run dev        # Start development server
npm run lint       # ESLint code checking
npm run type-check # TypeScript compilation check

# Frontend
npm run dev        # Start Next.js development server
npm run build      # Production build
npm run lint       # ESLint + TypeScript checking
```

## 🔧 Configuration Details

### Claude Code Settings (`.claude/`)
- **Project awareness**: Automatic context loading
- **Memory management**: Session history and task continuity  
- **Code quality**: Established patterns and standards
- **Troubleshooting**: Documented solutions for common issues

### VS Code Settings (`.vscode/`)
- **Format on save**: Prettier integration
- **TypeScript support**: Enhanced development experience
- **Consistent formatting**: Shared code style across team

### GitHub Copilot (`copilot-chat.json`)
- **Context-aware suggestions**: Project-specific AI assistance
- **Code completion**: Enhanced productivity features

## 📚 Project Documentation

The `CLAUDE.md` file contains comprehensive documentation including:

- **Project Overview**: Architecture and technology stack
- **Session History**: Development progress and completed features
- **Troubleshooting Guide**: Solutions for common development issues
- **Code Quality Standards**: Established patterns and best practices
- **Workflow Optimization**: Development efficiency protocols

## 🔄 Keeping Configuration Updated

When making changes to project configuration:

1. **Update this repository** with new configuration files
2. **Document changes** in CLAUDE.md
3. **Notify team members** to pull latest configuration
4. **Test configuration** across different development environments

## 🤝 Contributing

1. Fork this repository
2. Make configuration improvements
3. Test in your development environment
4. Submit pull request with detailed description
5. Update documentation if needed

## 📞 Support

For questions about configuration or development setup:

- **Documentation**: Refer to `CLAUDE.md` for comprehensive project information
- **Issues**: Create GitHub issues for configuration problems
- **Development**: Use Claude Code AI assistant with provided project context

## 🏷️ Version History

- **v1.0**: Initial configuration repository setup
- **Project Status**: Active development with production-ready features
- **Last Updated**: 2025-01-03

---

> **Note**: This configuration repository supports the JDN DP LogBook maritime operations management system. Ensure you have access to both dp-backend and dp-web repositories for complete development environment setup.