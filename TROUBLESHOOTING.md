# JDN DP LogBook - Troubleshooting Guide

## 🔧 TypeScript Issues & Solutions

### exactOptionalPropertyTypes Compliance
- **Problem**: TypeScript error with optional properties when exactOptionalPropertyTypes is enabled
- **Common Error**: `Type 'X' is not assignable to type 'Y'` with optional properties
- **Solution Pattern**: Use conditional property assignment instead of optional interfaces
- **Example Fix** (Session 6):
  ```typescript
  // ❌ Don't use optional when value is always provided
  interface UpdateProfileRequest {
    id?: number;
  }

  // ✅ Use required when value is guaranteed
  interface UpdateProfileRequest {
    id: number;
  }
  ```
- **Files Affected**: profiles.ts, form interfaces, API types
- **Quick Check**: Look for `id?: number` patterns where `id` is always provided

### Schema Method Issues (Zod)
- **Problem**: `.pick()` method not available on refined Zod schemas
- **Common Error**: `welcomeFormSchema.pick is not a function`
- **Root Cause**: Zod refinements return different schema types that don't support `.pick()`
- **Solution**: Use base schema for picking, apply refinements separately
- **Example Fix** (Session 2):
  ```typescript
  // ❌ Don't pick from refined schema
  const pickedSchema = welcomeFormSchema.refine(...).pick({ field: true })

  // ✅ Pick from base, then refine
  const baseSchema = z.object({ field: z.string() })
  const pickedSchema = baseSchema.pick({ field: true }).refine(...)
  ```
- **Files Affected**: Schema files in features/*/schemas/
- **Prevention**: Always separate base schemas from refinements

### User Role Type Safety
- **Problem**: `user?.role?.toLowerCase()` type errors when role can be string or object
- **Common Error**: Property 'toLowerCase' does not exist on type 'Role | string'
- **Solution**: Type guard for both string and object role types
- **Example Fix** (Session 9):
  ```typescript
  // ✅ Handle both role formats
  const roleString = typeof user.role === 'string' ? user.role : user.role?.name || '';
  const isOperator = roleString.toLowerCase() === 'operator';
  ```
- **Files Affected**: useWelcomeForm.ts, authentication logic
- **Root Cause**: Backend inconsistency in role format (string vs object)

## 🎨 CSS & Styling Issues

### Shadow Rendering in Light Mode
- **Problem**: Box shadows not visible in light mode due to CSS conflicts
- **Common Causes**:
  - Background color overriding transparency: `bg-gray-100` vs `bg-transparent`
  - Parent container `overflow-hidden` clipping shadows
  - Insufficient shadow opacity for light backgrounds
- **Solution Pattern** (Session 4):
  ```css
  /* ❌ Avoid background conflicts */
  .shadow-container {
    @apply bg-gray-100 shadow-lg;
  }

  /* ✅ Use transparent backgrounds for shadows */
  .shadow-container {
    @apply bg-transparent shadow-lg;
  }
  ```
- **Files Affected**: ProfileEditForm.tsx, form sections with shadows
- **Quick Fix**: Remove `bg-*` classes from shadow containers, increase shadow opacity

### Form Transition Issues
- **Problem**: CSS transitions causing layout shifts or visual glitches
- **Common Issues**:
  - Height animations without proper overflow handling
  - Spacing collapse between conditional sections
  - Pointer events active during transitions
- **Solution Pattern** (Session 8):
  - Use `overflow-hidden` only during transitions
  - Apply negative margins for space collapse: `-my-2` to `-my-5`
  - Disable pointer events with `pointer-events-none` during animation
- **Files Affected**: ProfileEditForm.tsx, conditional form sections
- **Performance**: Stagger transition durations (300-500ms) to prevent visual chaos

## 📡 API & Backend Integration

### Profile Logbooks Migration Pattern
- **Problem**: Transitioning from single logbook to multiple logbooks per profile
- **Database Migration Steps**:
  1. Create `profile_logbooks` junction table
  2. Migrate existing `logbook_type_id` data to new table
  3. Remove `logbook_type_id` column from profiles table
- **Frontend Updates Required**:
  - Replace single dropdown with LogbooksSection component
  - Update form validation schemas
  - Handle multiple logbook CRUD operations
- **TypeScript Fixes** (Session 9):
  - Replace `isLogbookTypeSelected` with `hasLogbooks` logic
  - Update conditional rendering based on `profileLogbooks.length > 0`
- **Files Affected**: ProfileEditForm.tsx, profiles controller, database schema

### React Hook Dependencies
- **Problem**: Missing dependencies in useCallback/useEffect causing stale closures
- **Common Warning**: `React Hook useCallback has a missing dependency`
- **Solution**: Always include all referenced variables in dependency arrays
- **Example Fix** (Session 9):
  ```typescript
  // ✅ Include fileToBase64 in dependencies
  const handleFileUpload = useCallback(async (file: File) => {
    const base64 = await fileToBase64(file);
    // ... rest of logic
  }, [fileToBase64]); // Add missing dependency
  ```
- **Files Affected**: Custom hooks, form handlers, async operations

## 🔐 Security & Authentication

### Session Management Security
- **Problem**: Client-side session storage using localStorage/sessionStorage
- **Security Risk**: XSS attacks can access client-side stored tokens
- **Current Status**: CRITICAL - Still using localStorage in authService.ts:93-112
- **Required Fix**: Migrate to HTTP-only cookies
- **Implementation Steps**:
  1. Update backend to set HTTP-only cookie on login
  2. Remove localStorage usage from authService.ts
  3. Update API calls to rely on cookie authentication
  4. Implement proper CSRF protection
- **Files Affected**: authService.ts, login API endpoints, authentication middleware

## 🔄 Development Workflow Issues

### Component Naming Inconsistencies
- **Problem**: Mixed snake_case and camelCase in component filenames
- **Current Issues**:
  - `avatarUser.tsx` should be `UserAvatar.tsx`
  - Form components mix `formElements` and `form_elements`
- **Standard**: Use PascalCase for components, camelCase for utilities
- **Batch Fix Strategy**: Use global find/replace with import path updates

### Component Duplication
- **Problem**: Multiple similar components scattered across codebase
- **Current Issues**: 6+ button components (DeleteButton, CancelButton, EditButton, etc.)
- **Consolidation Strategy**:
  1. Create base Button component with variant props
  2. Replace scattered components with variant usage
  3. Update all import statements
- **Files Affected**: Multiple button components, their usage sites

## 🧪 Testing & Verification

### Mandatory Verification Protocol
- **When to Verify**: Before marking any task as "completed"
- **Tools Required**: Glob + Read tools for file inspection
- **Evidence Standard**: Include file paths and line numbers
- **Risk Levels**:
  - **CRITICAL**: Security, authentication → ALWAYS verify
  - **HIGH**: Type safety, API integrations → Verify when possible
  - **MEDIUM/LOW**: UI/styling → Sample verification

### Session Start Audit Process
1. Review last 2-3 "completed" items from previous sessions
2. Use Read tool to verify claimed implementations exist
3. Document any discrepancies in Session Corrections Log
4. Update status with accurate indicators (🚨 ⚠️ ✅ ❌)

## 🚀 Performance Optimization

### React Hook Optimization
- **Missing Patterns**: useCallback/useMemo in form handlers
- **Common Issues**: Object recreation in dependency arrays
- **Solution**: Wrap functions in useCallback, expensive calculations in useMemo
- **Files Needing Optimization**: Form hooks, data transformation logic

### API Caching Architecture
- **Current State**: No caching library (React Query/SWR) implemented
- **Impact**: Repeated API calls, poor user experience
- **Implementation Priority**: Phase 3 - after critical security fixes
- **Strategy**: Evaluate React Query vs SWR based on project needs

## 🔍 Quick Reference Commands

### File Structure Discovery
```bash
# Find component patterns
find src -name "*.tsx" -type f | grep -i button

# Locate schema files
find src -path "*/schemas/*" -name "*.ts"

# Check for remaining any types
grep -r "any" src/ --include="*.ts" --include="*.tsx"
```

### Verification Commands
```bash
# Check package.json for dependencies
cat package.json | grep -A5 -B5 "dependencies"

# Find localStorage usage
grep -r "localStorage\|sessionStorage" src/

# Verify TypeScript compilation
npm run type-check || npx tsc --noEmit
```

### Development Server Status
- **Backend**: localhost:4000 (check with curl or browser)
- **Frontend**: localhost:3000/3001 (varies by configuration)
- **Database**: Prisma Studio available via `npx prisma studio`