# {项目名称} 前端架构文档

## 目录

{ 如果添加或删除了章节和子章节，请更新此内容 }

- [介绍](#介绍)
- [整体前端理念和模式](#整体前端理念和模式)
- [详细前端目录结构](#详细前端目录结构)
- [组件分解和实现细节](#组件分解和实现细节)
  - [组件命名和组织](#组件命名和组织)
  - [组件规范模板](#组件规范模板)
- [状态管理深入](#状态管理深入)
  - [存储结构/切片](#存储结构切片)
  - [关键选择器](#关键选择器)
  - [关键操作/归约器/异步操作](#关键操作归约器异步操作)
- [API 交互层](#api-交互层)
  - [客户端/服务结构](#客户端服务结构)
  - [错误处理和重试（前端）](#错误处理和重试前端)
- [路由策略](#路由策略)
  - [路由定义](#路由定义)
  - [路由守卫/保护](#路由守卫保护)
- [构建、打包和部署](#构建打包和部署)
  - [构建过程和脚本](#构建过程和脚本)
  - [关键打包优化](#关键打包优化)
  - [部署到 CDN/托管](#部署到-cdn托管)
- [前端测试策略](#前端测试策略)
  - [组件测试](#组件测试)
  - [UI 集成/流程测试](#ui-集成流程测试)
  - [端到端 UI 测试工具和范围](#端到端-ui-测试工具和范围)
- [可访问性（AX）实现细节](#可访问性ax实现细节)
- [性能考虑](#性能考虑)
- [国际化（i18n）和本地化（l10n）策略](#国际化i18n和本地化l10n策略)
- [功能标志管理](#功能标志管理)
- [前端安全考虑](#前端安全考虑)
- [浏览器支持和渐进增强](#浏览器支持和渐进增强)
- [变更日志](#变更日志)

## 介绍

{ 本文档详细说明了 {项目名称} 前端的具体技术架构。它是对主 {项目名称} 架构文档和 UI/UX 规范的补充。本文档详细说明了前端架构，并**基于主 {项目名称} 架构文档（`docs/architecture.md` 或等效链接）中定义的基础决策**（例如，整体技术栈、CI/CD、主要测试工具）。**必须在此处明确说明与通用模式不同的前端特定扩展或偏差。**目标是提供清晰的前端开发蓝图，确保一致性、可维护性，并与整体系统设计和用户体验目标保持一致。 }

- **主架构文档链接（必需）：** {例如，`docs/architecture.md`}
- **UI/UX 规范链接（如果存在则必需）：** {例如，`docs/front-end-spec.md`}
- **主要设计文件链接（Figma、Sketch 等）（如果存在则必需）：** {来自 UI/UX 规范}
- **已部署的 Storybook / 组件展示链接（如果适用）：** {URL}

## 整体前端理念和模式

{ 描述为前端选择的核心架构决策和模式。这应该与主架构文档中的"确定的技术栈选择"保持一致，并考虑整体系统架构的影响（例如，monorepo 与 polyrepo，后端服务结构）。 }

- **框架和核心库：** {例如，React 18.x 与 Next.js 13.x，Angular 16.x，Vue 3.x 与 Nuxt.js}。**这些来自主架构文档中的"确定的技术栈选择"**。本节详细说明这些选择如何具体应用于前端。
- **组件架构：** {例如，原子设计原则，展示型与容器型组件，使用特定的组件库如 Material UI，Tailwind CSS 用于样式方法。指定选择的方法和任何关键库。}
- **状态管理策略：** {例如，Redux Toolkit，Zustand，Vuex，NgRx。简要描述整体方法 - 全局存储，功能存储，Context API 使用。**参考主架构文档并在"状态管理深入"部分详细说明。**}
- **数据流：** {例如，单向数据流（Flux/Redux 模式），React Query/SWR 用于服务器状态。描述数据如何获取、缓存、传递给组件和更新。}
- **样式方法：** **{选择的样式解决方案，例如，Tailwind CSS / CSS Modules / Styled Components}**。配置文件：{例如，`tailwind.config.js`，`postcss.config.js`}。关键约定：{例如，"Tailwind 的实用优先方法。自定义组件在 `src/styles/components.css` 中定义。主题扩展在 `tailwind.config.js` 的 `theme.extend` 下。对于 CSS Modules，文件与组件放在一起，例如，`MyComponent.module.css`。}
- **使用的关键设计模式：** {例如，Provider 模式，Hooks，高阶组件，用于 API 调用的服务模式，容器/展示型组件。这些模式必须一致应用。偏差需要理由和文档说明。}

## 详细前端目录结构

{ 提供表示前端应用程序特定文件夹结构的 ASCII 图（例如，在 `src/` 或 `app/` 内，或者如果是 monorepo 的一部分，则在专用的 `frontend/` 根目录内）。这应该详细说明架构文档中概述的主项目结构的前端部分。突出显示组织组件、页面/视图、服务、状态、样式、资源等的约定。对于每个关键目录，提供其用途的强制性一句话描述。}

### 示例 - 非规范性（适用于 React/Next.js 应用）

```plaintext
src/
├── app/                        # Next.js App Router：页面/布局/路由。必须包含路由段、布局和页面组件。
│   ├── (features)/             # 基于功能的路由组。必须将特定功能的相关路由分组。
│   │   └── dashboard/
│   │       ├── layout.tsx      # 特定于仪表板功能路由的布局。
│   │       └── page.tsx        # 仪表板路由的入口页面组件。
│   ├── api/                    # API 路由（如果使用 Next.js 后端功能）。必须包含客户端调用的后端处理程序。
│   ├── globals.css             # 全局样式。必须包含基础样式、CSS 变量定义、Tailwind 基础/组件/实用工具。
│   └── layout.tsx              # 整个应用程序的根布局。
├── components/                 # 共享/可重用的 UI 组件。
│   ├── ui/                     # 基础 UI 元素（按钮、输入框、卡片）。必须仅包含通用的、可重用的、展示型 UI 元素，通常从设计系统映射。不得包含业务逻辑。
│   │   ├── Button.tsx
│   │   └── ...
│   ├── layout/                 # 布局组件（页眉、页脚、侧边栏）。必须包含构建页面布局的组件，而不是特定页面内容。
│   │   ├── Header.tsx
│   │   └── ...
│   └── (feature-specific)/     # 特定于功能的组件，但可能在该功能内重用。这是与 features/ 目录中共同定位的替代方案。
│       └── user-profile/
│           └── ProfileCard.tsx
├── features/                   # 特定于功能的逻辑、钩子、非全局状态、服务和仅由该功能使用的组件。
│   └── auth/
│       ├── components/         # 仅由 auth 功能使用的组件。不得被其他功能导入。
│       ├── hooks/              # 特定于 'auth' 功能的自定义 React 钩子。可在功能间重用的钩子属于 `src/hooks/`。
│       ├── services/           # 特定于 'auth' 功能的 API 交互或编排。
│       └── store.ts            # 特定于功能的状态切片（例如，Redux slice），如果不是全局存储的一部分或本地状态复杂。
├── hooks/                      # 全局/可共享的自定义 React 钩子。必须是通用的，可由多个功能/组件使用。
│   └── useAuth.ts
├── lib/ / utils/             # 工具函数、辅助函数、常量。必须包含纯函数和常量，除非明确命名（例如，`react-helpers.ts`），否则不得有副作用或框架特定代码。
│   └── utils.ts
├── services/                   # 全局 API 服务客户端或 SDK 配置。必须定义基础 API 客户端实例和核心数据获取/变更服务。
│   └── apiClient.ts
├── store/                      # 全局状态管理设置（例如，Redux store，Zustand store）。
│   ├── index.ts                # 主存储配置和导出。
│   ├── rootReducer.ts          # 如果使用 Redux，则为根归约器。
│   └── (slices)/               # 全局状态切片目录（如果不在功能中共同定位）。
├── styles/                     # 全局样式、主题配置（如果不使用 `globals.css` 或类似文件，或用于特定样式系统如 SCSS 部分）。
└── types/                      # 全局 TypeScript 类型定义/接口。必须包含在多个功能/模块间共享的类型。
    └── index.ts
```

### 关于前端结构的说明：

{ 解释结构背后的任何特定约定或理由。例如，"如果组件不是全局可重用的，则与它们的功能共同定位以提高模块化。" AI 代理必须严格遵守这个定义的结构。新文件必须根据这些描述放在适当的目录中。 }

## 组件分解和实现细节

{ 本节概述了定义 UI 组件的约定和模板。大多数特定于功能的组件的详细规范将随着用户故事的实现而出现。每当识别出需要开发的新组件时，AI 代理必须遵循下面的"组件规范模板"。 }

### 组件命名和组织

- **组件命名约定：** **{例如，文件和组件名称使用 PascalCase：`UserProfileCard.tsx`}**。所有组件文件必须遵循此约定。
- **组织：** {例如，"全局可重用组件在 `src/components/ui/` 或 `src/components/layout/` 中。特定于功能的组件在其功能目录中共同定位，例如，`src/features/feature-name/components/`。参考详细前端目录结构。}

### 组件规范模板

{ 对于从 UI/UX 规范和设计文件（Figma）中识别的每个重要 UI 组件，必须提供以下详细信息。为每个组件重复此子节。详细程度必须足以让 AI 代理或开发人员以最小的歧义实现它。 }

#### 组件：`{ComponentName}`（例如，`UserProfileCard`，`ProductDetailsView`）

- **用途：** {简要描述此组件的功能和它在 UI 中的角色。必须清晰简洁。}
- **源文件：** {例如，`src/components/user-profile/UserProfileCard.tsx`。必须是确切的路径。}
- **视觉参考：** {链接到特定的 Figma 框架/组件，或 Storybook 页面。必需。}
- **属性（Props）：**
  { 列出组件接受的每个属性。对于每个属性，必须填写表中的所有列。 }
  | 属性名称 | 类型 | 必需？ | 默认值 | 描述 |
  | :-------------- | :---------------------------------------- | :-------- | :------------ | :--------------------------------------------------------------------------------------------------------- |
  | `userId` | `string` | 是 | 无 | 要显示的用户 ID。必须是有效的 UUID。 |
  | `avatarUrl` | `string \| null` | 否 | `null` | 用户头像图片的 URL。如果提供，必须是有效的 HTTPS URL。 |
  | `onEdit` | `() => void` | 否 | 无 | 触发编辑操作时的回调函数。 |
  | `variant` | `'compact' \| 'full'` | 否 | `'full'` | 控制卡片的显示模式。 |
  | `{anotherProp}` | `{特定原始类型、导入的类型或内联接口/类型定义}` | {是/否} | {如果有} | {必须明确说明属性的用途和任何约束，例如，'必须是正整数。'} |
- **内部状态（如果有）：**
  { 描述组件管理的任何重要内部状态。仅列出不是从属性或全局状态派生的状态。如果状态复杂，考虑是否应该由自定义钩子或全局状态解决方案管理。 }
  | 状态变量 | 类型 | 初始值 | 描述 |
  | :-------------- | :-------- | :------------ | :----------------------------------------------------------------------------- |
  | `isLoading` | `boolean` | `false` | 跟踪组件数据是否正在加载。 |
  | `{anotherState}`| `{type}` | `{value}` | {状态变量及其用途的描述。} |
- **关键 UI 元素/结构：**
  { 提供表示组件 DOM 的伪 HTML 或 JSX 类结构。如果适用，包括关键条件渲染逻辑。**此结构决定了 AI 代理的主要输出。** }
  ```html
  <div>
    <!-- 主卡片容器，具有特定类，例如，基于 variant 属性的 styles.cardFull 或 styles.cardCompact -->
    <img src="{avatarUrl || defaultAvatar}" alt="用户头像" class="{styles.avatar}" />
    <h2>{userName}</h2>
    <p class="{variant === 'full' ? styles.emailFull : styles.emailCompact}">{userEmail}</p>
    {variant === 'full' && onEdit &&
    <button onClick="{onEdit}" class="{styles.editButton}">编辑</button>}
  </div>
  ```
- **处理/触发的事件：**
  - **处理：** {例如，编辑按钮上的 `onClick`（触发 `onEdit` 属性）。}
  - **触发：** {如果组件触发未由属性覆盖的自定义事件/回调，请使用其确切签名描述它们。例如，`onFollow: (payload: { userId: string; followed: boolean }) => void`}
- **触发的操作（副作用）：**
  - **状态管理：** {例如，"从 `src/store/slices/userSlice.ts` 分发 `userSlice.actions.setUserName(newName)`。操作负载必须匹配定义的操作创建器。" 或者 "从本地 `useReducer` 钩子调用 `updateUserProfileOptimistic(newData)`。"}
  - **API 调用：** {指定从"API 交互层"调用的服务/函数。例如，"在挂载时调用 `src/services/userService.ts` 中的 `userService.fetchUser(userId)`。请求负载：`{ userId }`。成功响应填充内部状态 `userData`。错误响应分发 `uiSlice.actions.showErrorToast({ message: '加载用户详情失败' })`。"}
- **样式说明：**
  - {必须引用特定的设计系统组件名称（例如，"使用 UI 库中的 `<Button variant='primary'>`"）或指定要应用的 Tailwind CSS 类/CSS 模块类名（例如，"容器使用 `p-4 bg-white rounded-lg shadow-md`。标题使用 `text-xl font-semibold`。"）或指定要应用的 SCSS 自定义组件类（例如，"容器使用 `@apply p-4 bg-white rounded-lg shadow-md`。标题使用 `@apply text-xl font-semibold`。"）。必须描述基于属性或状态的任何动态样式逻辑。如果使用 Tailwind CSS，列出主要实用类或自定义组件类的 `@apply` 指令。AI 代理应该优先考虑简单情况的直接实用类使用，并为复杂的样式模式提出可重用的组件类/React 组件。}
- **可访问性说明：**
  - {必须列出特定的 ARIA 属性及其值（例如，`aria-label="用户资料卡片"`，`role="article"`），必需的键盘导航行为（例如，"Tab 导航到头像、名称、电子邮件，然后是编辑按钮。编辑按钮可聚焦并通过 Enter/Space 激活。"），以及任何焦点管理要求（例如，"如果此组件打开模态框，焦点必须被困在内部。模态框关闭时，焦点返回到触发元素。"）。}

---

_为每个重要组件重复上述模板。_

---

## 状态管理深入

{ 本节扩展了状态管理策略。**请参考主架构文档以获取最终的状态管理解决方案选择。** }

- **选定方案：** {例如，Redux Toolkit，Zustand，Vuex，NgRx——如主架构文档所定义。}
- **状态存放决策指南：**
  - **全局状态（如 Redux/Zustand）：** 跨多个无关组件共享的数据；跨路由持久化的数据；通过 reducer/thunk 管理的复杂状态逻辑。**必须用于会话数据、用户偏好、全局通知。**
  - **React Context API：** 主要在特定组件子树中传递的状态（如主题、表单上下文）。比全局状态更简单，更新频率较低。**适用于不适合 props 传递但也不需要全局的本地化状态。**
  - **本地组件状态（`useState`、`useReducer`）：** 仅在组件或其直接子组件中需要的 UI 特定状态（如表单输入值、下拉菜单开关状态）。**除非满足 Context 或全局状态的标准，否则应为默认选择。**

### 存储结构/切片

{ 描述全局状态的组织约定（如"每个需要全局状态的重要功能都将在 `src/features/[featureName]/store.ts` 中有自己的 Redux 切片"）。 }

- **核心切片示例（如 `sessionSlice` 在 `src/store/slices/sessionSlice.ts`）：**
  - **用途：** {管理用户会话、认证状态和全局可访问的基础用户信息。}
  - **状态结构（接口/类型）：**
    ```typescript
    interface SessionState {
      currentUser: { id: string; name: string; email: string; roles: string[] } | null
      isAuthenticated: boolean
      token: string | null
      status: 'idle' | 'loading' | 'succeeded' | 'failed'
      error: string | null
    }
    ```
  - **关键 reducer/操作（在 `createSlice` 内）：** {简要列出主要同步操作，如 `setCurrentUser`、`clearSession`、`setAuthStatus`、`setAuthError`。}
  - **异步 thunk（如有）：** {列出主要异步 thunk，如 `loginUserThunk`、`fetchUserProfileThunk`。}
  - **选择器（用 `createSelector` 记忆化）：** {列出主要选择器，如 `selectCurrentUser`、`selectIsAuthenticated`。}
- **功能切片模板（如 `{featureName}Slice` 在 `src/features/{featureName}/store.ts`）：**
  - **用途：** {当新功能需要自己的状态切片时填写。}
  - **状态结构（接口/类型）：** {由功能定义。}
  - **关键 reducer/操作（在 `createSlice` 内）：** {由功能定义。}
  - **异步 thunk（如有，使用 `createAsyncThunk` 定义）：** {由功能定义，遵循类似的 pending、fulfilled、rejected 状态处理模式，包括 API 调用和状态更新。}
  - **导出：** {所有操作和选择器必须导出。}

### 关键选择器

{ 列出任何核心、前置切片的重要选择器。对于新出现的功能切片，选择器将在切片中定义。**所有派生数据或组合多个状态片段的选择器必须使用 Reselect 的 `createSelector`（或其他状态库的等效方法）进行记忆化。** }

- **`selectCurrentUser`（来自 `sessionSlice`）：** {返回 `currentUser` 对象。}
- **`selectIsAuthenticated`（来自 `sessionSlice`）：** {返回 `isAuthenticated` 布尔值。}
- **`selectAuthToken`（来自 `sessionSlice`）：** {返回 `sessionSlice` 中的 `token`。}

### 关键操作/归约器/异步操作

{ 详细说明核心、前置切片的更复杂操作，尤其是异步 thunk 或 saga。每个 thunk 必须清楚定义其用途、参数、所调用的 API（参考 API 交互层），以及如何在 pending、fulfilled 和 rejected 状态下更新状态。}

- **核心操作/异步操作示例：`authenticateUser(credentials: AuthCredentials)`（在 `sessionSlice.ts`）：**
  - **用途：** {通过调用认证 API 并更新 `sessionSlice` 处理用户登录。}
  - **参数：** `credentials: { email: string; password: string }`
  - **分发流程（使用 Redux Toolkit `createAsyncThunk`）：**
    1. `pending` 时：分发 `sessionSlice.actions.setAuthStatus('loading')`。
    2. 调用 `authService.login(credentials)`（来自 `src/services/authService.ts`）。
    3. `fulfilled`（成功）时：分发 `sessionSlice.actions.setCurrentUser(response.data.user)`、`sessionSlice.actions.setToken(response.data.token)`、`sessionSlice.actions.setAuthStatus('succeeded')`。
    4. `rejected`（错误）时：分发 `sessionSlice.actions.setAuthError(error.message)`、`sessionSlice.actions.setAuthStatus('failed')`。
- **功能操作/异步操作模板：`{featureActionName}`（在 `{featureName}Slice.ts`）：**
  - **用途：** {为功能特定的异步操作填写。}
  - **参数：** {定义具体参数及类型。}
  - **分发流程（使用 `createAsyncThunk`）：** {由功能定义，遵循类似的 pending、fulfilled、rejected 状态处理模式，包括 API 调用和状态更新。}

## API 交互层

{ 描述前端如何与主架构文档中定义的后端 API 通信。 }

### 客户端/服务结构

- **HTTP 客户端设置：** {例如，在 `src/services/apiClient.ts` 中配置 Axios 实例。**必须**包含：基础 URL（从环境变量 `NEXT_PUBLIC_API_URL` 或等效变量获取），默认请求头（如 `Content-Type: 'application/json'`），用于自动注入认证令牌（从状态管理获取，如 `sessionSlice.token`）的拦截器，以及标准化的错误处理/规范化（见下文）。}
- **服务定义（示例）：**
  - **`userService.ts`（在 `src/services/userService.ts`）：**
    - **用途：** {处理与用户相关的所有 API 交互。}
    - **函数：** 每个服务函数必须具有明确的参数类型、返回类型（如 `Promise<User>`），以及 JSDoc/TSDoc 说明其用途、参数、返回值和任何特定的错误处理。它必须调用配置好的 HTTP 客户端（`apiClient`），并指定正确的端点、方法和负载。
      - `fetchUser(userId: string): Promise<User>`
      - `updateUserProfile(userId: string, data: UserProfileUpdateDto): Promise<User>`
  - **`productService.ts`（在 `src/services/productService.ts`）：**
    - **用途：** {...}
    - **函数：** {...}

### 错误处理和重试（前端）

- **全局错误处理：** {如何全局捕获 API 错误？（例如，通过 `apiClient.ts` 中的 Axios 响应拦截器）。如何呈现/记录错误？（例如，分发 `uiSlice.actions.showGlobalErrorBanner({ message: error.message })`，将详细错误记录到控制台/监控服务）。是否存在全局错误状态？（例如，`uiSlice.error`）。}
- **特定错误处理：** {组件可以本地处理特定 API 错误以提供更上下文相关的反馈（例如，在表单字段上显示内联消息："无效的电子邮件地址"）。如果偏离全局处理，必须在组件规范中记录。}
- **重试逻辑：** {是否实现了客户端重试逻辑（例如，使用 `axios-retry` 与 `apiClient`）？如果是，请指定配置：最大重试次数（例如，3），重试条件（例如，网络错误、5xx 服务器错误），重试延迟（例如，指数退避）。**仅适用于幂等请求（GET、PUT、DELETE）。**}

## 路由策略

{ 详细说明前端应用程序如何处理导航和路由。 }

- **路由库：** {例如，React Router，Next.js App Router，Vue Router，Angular Router。根据主架构文档。}

### 路由定义

{ 列出应用程序的主要路由以及每个路由渲染的主要组件/页面。 }

| 路径模式               | 组件/页面（`src/app/...` 或 `src/pages/...`）         | 保护                                      | 备注                                  |
| :--------------------- | :---------------------------------------------------- | :---------------------------------------- | :------------------------------------ |
| `/`                    | `app/page.tsx` 或 `pages/HomePage.tsx`                | `Public`                                  |                                       |
| `/login`               | `app/login/page.tsx` 或 `pages/LoginPage.tsx`         | `Public`（如果已认证则重定向）            | 如果已认证，则重定向到 `/dashboard`。 |
| `/dashboard`           | `app/dashboard/page.tsx` 或 `pages/DashboardPage.tsx` | `Authenticated`                           |                                       |
| `/products`            | `app/products/page.tsx`                               | `Public`                                  |                                       |
| `/products/:productId` | `app/products/[productId]/page.tsx`                   | `Public`                                  | 参数：`productId`（字符串）           |
| `/settings/profile`    | `app/settings/profile/page.tsx`                       | `Authenticated`，`Role:[USER]`            | 基于角色的保护示例。                  |
| `{anotherRoute}`       | `{ComponentPath}`                                     | `{Public/Authenticated/Role:[ROLE_NAME]}` | {备注，参数名称和类型}                |

### 路由守卫/保护

- **认证守卫：** {描述如何基于认证状态保护路由。**指定确切的 HOC、钩子、布局或中间件机制及其位置**（例如，`src/guards/AuthGuard.tsx`，或 Next.js 中间件在 `middleware.ts`）。逻辑必须使用来自 `sessionSlice`（或等效）的认证状态。未认证用户尝试访问受保护路由时，必须重定向到 `/login`（或指定的登录路径）。}
- **授权守卫（如适用）：** {描述如何基于用户角色或权限保护路由。**指定确切的机制**，类似于认证守卫。未授权用户（已认证但缺乏权限）必须显示"禁止访问"页面或重定向到安全页面。}

## 构建、打包和部署

{ 与主架构文档中的"基础设施和部署概述"互补的前端构建和部署过程的详细信息。 }

### 构建过程和脚本

- **关键构建脚本（来自 `package.json`）：** {例如，`"build": "next build"`。它们做什么？指向 `package.json` 脚本。`"dev": "next dev"`，`"start": "next start"`。} **AI 代理不得生成硬编码环境特定值的代码。所有此类值必须通过定义的环境配置机制访问。** 指定确切的文件和访问方法。
- **环境配置管理：** {如何为不同环境（开发、测试、生产）管理 `process.env.NEXT_PUBLIC_API_URL`（或等效变量，如 `import.meta.env.VITE_API_URL`）？（例如，Next.js/Vite 的 `.env`、`.env.development`、`.env.production` 文件；通过 CI 变量在构建时注入）。指定确切的文件和访问方法。}

### 关键打包优化

- **代码分割：** {如何实现/确保？（例如，"Next.js/Vite 自动处理基于路由的代码分割。对于组件级代码分割，必须使用动态导入 `React.lazy(() => import('./MyComponent'))` 或 `import('./heavy-module')` 用于非关键大型组件/库。"）}
- **Tree Shaking：** {如何实现/确保？（例如，"通过现代构建工具如 Webpack/Rollup（Next.js/Vite 使用）在使用 ES 模块时确保。避免在共享库中使用有副作用的导入。"）}
- **懒加载（组件、图像等）：** {懒加载策略。（例如，"组件：使用 `React.lazy` 和 `Suspense`。图像：使用框架特定的 Image 组件，如 `next/image`，默认处理懒加载，或标准 `<img>` 标签的 `loading='lazy'` 属性。"）}
- **压缩和压缩：** {由构建工具处理（例如，Webpack/Terser，Vite/esbuild）？指定是否需要特定配置。压缩（例如，Gzip，Brotli）通常由托管平台/CDN 处理。}

### 部署到 CDN/托管

- **目标平台：** {例如，Vercel，Netlify，AWS S3/CloudFront，Azure Static Web Apps。根据主架构文档。}
- **部署触发：** {例如，通过 GitHub Actions 推送到 `main` 分支（参考主 CI/CD 管道）。}
- **资源缓存策略：** {如何缓存静态资源？（例如，"不可变资源（带有内容哈希的 JS/CSS 包）具有 `Cache-Control: public, max-age=31536000, immutable`。HTML 文件具有 `Cache-Control: no-cache` 或短 max-age（例如，`public, max-age=0, must-revalidate`）以确保用户获取最新的入口点。通过 {托管平台设置 / `next.config.js` 头 / CDN 规则} 配置。}

## 前端测试策略

{ 本节详细说明了主架构文档中的"测试策略"，重点关注前端特定方面。**请参考主架构文档以获取测试工具的最终选择。** }

- **链接到主整体测试策略：** {参考主 `docs/architecture.md#overall-testing-strategy` 或等效。}

### 组件测试

- **范围：** {在隔离环境中测试单个 UI 组件（类似于组件的单元测试）。}
- **工具：** {例如，React Testing Library 与 Jest，Vitest，Vue Test Utils，Angular Testing Utilities。根据主架构文档。}
- **重点：** {使用各种 props 渲染，用户交互（使用 `fireEvent` 或 `userEvent` 的点击、输入更改），事件触发，基本内部状态更改。**快照测试必须谨慎使用，并有明确理由（例如，用于非常稳定、纯展示型、具有复杂 DOM 结构的组件）；优先使用显式断言。**}
- **位置：** {例如，`*.test.tsx` 或 `*.spec.tsx` 与组件共同定位，或在 `__tests__` 子目录中。}

### 功能/流程测试（UI 集成）

- **范围：** {测试多个组件如何交互以实现页面内的小型用户流程或功能，可能模拟 API 调用或全局状态管理。例如，测试功能内的完整表单提交，包括验证和与模拟服务层的交互。}
- **工具：** {与组件测试相同（例如，React Testing Library 与 Jest/Vitest），但设置更复杂，涉及路由、状态、API 调用的模拟提供者。}
- **重点：** {组件之间的数据流，基于交互的条件渲染，功能内的导航，与模拟服务/状态的集成。}

### 端到端 UI 测试工具和范围

- **工具：** {从主测试策略中重申，例如，Playwright，Cypress，Selenium。}
- **范围（前端重点）：** {定义 3-5 个必须由 E2E UI 测试覆盖的关键用户旅程，从 UI 角度出发，例如，"用户注册和登录流程"，"将商品添加到购物车并进入结账页面摘要"，"提交复杂的多步骤表单并验证成功 UI 状态和数据持久性（通过 API 模拟或测试后端）"。}
- **UI 测试数据管理：** {如何为 UI E2E 测试提供一致的测试数据？（例如，API 模拟层如 MSW，后端种子脚本，专用测试账户）。}

## 可访问性（AX）实现细节

{ 基于 UI/UX 规范中的 AX 要求，详细说明如何从技术上实现这些要求。 }

- **语义 HTML：** {强调使用正确的 HTML5 元素。**AI 代理必须优先使用语义元素（例如，`<nav>`、`<button>`、`<article>`），而不是在存在具有正确语义的原生元素时使用带有 ARIA 角色的通用 `<div>`/`<span>`。**}
- **ARIA 实现：** {指定常见自定义组件及其所需的 ARIA 模式（例如，"自定义选择下拉菜单必须遵循 ARIA Combobox 模式，包括 `aria-expanded`、`aria-controls`、`role='combobox'` 等。自定义标签必须遵循 ARIA Tabbed Interface 模式。"）。链接到 ARIA 创作实践指南（APG）以供参考。}
- **键盘导航：** {确保所有交互元素可通过键盘聚焦和操作。焦点顺序必须合理。自定义组件必须按照 ARIA APG 实现键盘交互模式（例如，单选组/滑块的箭头键）。}
- **焦点管理：** {如何在模态框、动态内容更改、路由转换中管理焦点？（例如，"模态框必须捕获焦点。打开模态框时，焦点移动到第一个可聚焦元素或模态框容器。关闭时，焦点返回到触发元素。路由更改应将焦点移动到新页面的主要内容区域或 H1。"）}
- **AX 测试工具：** {例如，Axe DevTools 浏览器扩展，Lighthouse 可访问性审计。**自动 Axe 扫描（例如，使用 `jest-axe` 进行组件测试，或 Playwright/Cypress Axe 集成进行 E2E 测试）必须集成到 CI 管道中，并在违反 WCAG AA（或指定级别）时使构建失败。** 手动测试程序：{列出关键手动检查，例如，所有交互元素的仅键盘导航，关键用户流程的屏幕阅读器测试（例如，NVDA/JAWS/VoiceOver）。}}

## 性能考虑

{ 突出前端特定的性能优化策略。 }

- **图像优化：** {格式（例如，WebP），响应式图像（`<picture>`、`srcset`），懒加载。}
  - 实现要求：{例如，"所有图像必须使用 Next.js 的 `<Image>` 组件（或等效的框架特定优化器）。图标使用 SVG。在支持的情况下优先使用 WebP 格式。"}
- **代码分割和懒加载（如需要，从构建部分重申）：** {它如何影响感知性能。}
  - 实现要求：{例如，"Next.js 自动处理基于路由的代码分割。必须使用动态导入 `import()` 进行组件级懒加载。"}
- **最小化重渲染：** {技术如 `React.memo`、`shouldComponentUpdate`、优化选择器。}
  - 实现要求：{例如，"必须对频繁渲染且 props 相同的组件使用 `React.memo`。全局状态的选择器必须记忆化（例如，使用 Reselect）。避免在渲染方法中直接传递新的对象/数组字面量或内联函数作为 props，这可能导致不必要的重渲染。"}
- **防抖/节流：** {用于搜索输入或窗口调整大小等事件处理程序。}
  - 实现要求：{例如，"对指定的事件处理程序使用 `lodash.debounce` 或 `lodash.throttle` 等工具。定义防抖/节流等待时间。"}
- **虚拟化：** {用于长列表或大型数据集（例如，React Virtualized，TanStack Virtual）。}
  - 实现要求：{例如，"如果观察到性能下降，必须对任何渲染超过 {N，例如，100} 个项目的列表使用虚拟化。"}
- **缓存策略（客户端）：** {使用浏览器缓存，用于 PWA 功能的 Service Worker（如适用）。}
  - 实现要求：{例如，"如果使用 PWA，配置 Service Worker 以缓存应用程序外壳和关键静态资源。利用 HTTP 缓存头为其他资源缓存，如部署部分所定义。"}
- **性能监控工具：** {例如，Lighthouse，WebPageTest，浏览器 DevTools 性能选项卡。指定哪些是主要的，以及 CI 中的任何自动检查。}

## 国际化（i18n）和本地化（l10n）策略

{本节定义了支持多种语言和区域差异的策略（如适用）。如不适用，请说明"目前该项目不需要国际化。"}

- **需求级别：** {例如，不需要，需要特定语言[列出它们]，完全国际化以支持未来扩展。}
- **选定的 i18n 库/框架：** {例如，`react-i18next`，`vue-i18n`，`ngx-translate`，框架原生解决方案如 Next.js i18n 路由。指定确切的库/机制。}
- **翻译文件结构和格式：** {例如，每个功能每种语言的 JSON 文件（`src/features/{featureName}/locales/{lang}.json`），或全局文件（`public/locales/{lang}.json`）。定义确切的路径和格式（例如，扁平 JSON，嵌套 JSON）。}
- **翻译键命名约定：** {例如，`featureName.componentName.elementText`，`common.submitButton`。必须是一个清晰、一致且有文档的模式。}
- **添加新可翻译字符串的过程：** {例如，"AI 代理必须将新键添加到默认语言文件（例如，`en.json`），并使用 i18n 库的函数/组件（例如，`<Trans>` 组件，`t()` 函数）来渲染文本。键不得在运行时以阻止静态分析的方式动态构建。"}
- **处理复数形式：** {指定方法/语法，例如，通过所选库使用 ICU 消息格式（例如，`t('key', { count: N })`）。}
- **日期、时间和数字格式化：** {指定 i18n 库是否处理此问题，或者是否应使用另一个库（例如，具有区域支持的 `date-fns-tz`，直接使用 `Intl` API）以及每个区域应使用的特定格式/样式。}
- **默认语言：** {例如，`en-US`}
- **语言切换机制（如适用）：** {用户如何更改语言并持久化？例如，"通过语言选择器组件更新全局状态/cookie，并可能更改 URL 路由。"}

## 功能标志管理

{本节概述了如何管理有条件启用的功能。如不适用，请说明"功能标志目前不是该项目的主要架构关注点。"}

- **需求级别：** {例如，不需要，用于特定发布，作为开发工作流程的核心部分。}
- **选定的功能标志系统/库：** {例如，LaunchDarkly，Unleash，Flagsmith，使用环境变量或配置服务的自定义解决方案。指定确切的工具/方法。}
- **在代码中访问标志：** {例如，"通过自定义钩子 `useFeatureFlag('flag-name'): boolean` 或服务 `featureFlagService.isOn('flag-name')`。指定确切的接口、位置和服务的初始化/提供者。"}
- **标志命名约定：** {例如，`[SCOPE]_[FEATURE_NAME]_[TARGET_GROUP_OR_TYPE]`，例如，`CHECKOUT_NEW_PAYMENT_GATEWAY_ROLLOUT`，`USER_PROFILE_BETA_AVATAR_UPLOAD`。必须记录并一致应用。}
- **标志功能的代码结构：** {例如，"使用条件渲染（`{isFeatureEnabled && <NewComponent />}`）。对于较大功能，有条件地导入组件（使用标志检查的 `React.lazy`）或路由。避免在共享组件深处使用复杂的分支逻辑；优先在更高级别进行标志控制。"}
- **代码清理策略（标志退役后）：** {例如，"一旦标志完全推出（100% 用户）并被视为永久，或完全移除，所有条件逻辑、旧代码路径和标志本身必须在 {N，例如，2} 个迭代内从代码库中移除。这是一项强制性的技术债务项目。"}
- **测试标志功能：** {如何测试不同的标志变体？例如，"QA 团队使用调试面板切换标志。自动化 E2E 测试使用特定的标志配置运行。"}

## 前端安全考虑

{本节强调了强制性的前端特定安全实践，补充主架构文档。AI 代理必须遵守这些指南。}

- **跨站脚本（XSS）防护：**
  - 框架依赖：{例如，"必须依赖 React 的 JSX 自动转义来渲染动态内容。除非内容经过明确清理，否则必须避免使用 Vue 的 `v-html`。"}
  - 显式清理：{如果必须直接操作 DOM（强烈不推荐），使用 {特定的清理库/函数，如 DOMPurify}。指定其配置。}
  - 内容安全策略（CSP）：{是否实施了 CSP？如何实施？例如，"CSP 通过后端/CDN 设置的 HTTP 头强制执行，如主架构文档所定义。如果不允许 `unsafe-inline`，前端可能需要确保内联脚本使用 nonce。" 如果可用，链接到 CSP 定义。}
- **跨站请求伪造（CSRF）防护（如适用于基于会话的认证）：**
  - 机制：{例如，"后端使用同步器令牌模式。如果 HTTP 客户端或表单未自动处理，前端确保在状态更改请求中包含令牌。" 参考主架构文档了解后端详情。}
- **安全令牌存储和处理（用于客户端令牌，如 JWT）：**
  - 存储机制：{**必须指定确切的机制**：例如，通过状态管理在内存中（例如，Redux/Zustand 存储，标签关闭时清除），`HttpOnly` cookie（如果后端设置且前端不需要读取），`sessionStorage`。**强烈不推荐使用 `localStorage` 存储令牌。**}
  - 令牌刷新：{描述客户端参与，例如，"`apiClient.ts` 中的拦截器处理 401 错误以触发令牌刷新端点。"}
- **第三方脚本安全：**
  - 策略：{例如，"所有第三方脚本（分析、广告、小部件）必须经过必要性 and 安全审查。异步加载脚本（`async/defer`）。"}
  - 子资源完整性（SRI）：{例如，"必须对所有从 CDN 加载的外部脚本和样式表使用 SRI 哈希，前提是资源稳定。"}
- **客户端数据验证：**
  - 目的：{例如，"客户端验证仅用于改善用户体验（即时反馈）。**所有关键数据验证必须在服务器端进行**（如主架构文档所定义）。"}
  - 实现：{例如，"使用 {表单库名称，如 Formik/React Hook Form} 进行表单验证。规则应适当反映服务器端验证。"}
- **防止点击劫持：**
  - 机制：{例如，"主要防御是后端/CDN 设置的 `X-Frame-Options` 或 `frame-ancestors` CSP 指令。前端代码不应依赖框架破坏脚本。"}
- **API 密钥暴露（用于客户端消费的服务）：**
  - 限制：{例如，"Google Maps（客户端 JS SDK）等服务 API 密钥必须通过服务提供商控制台进行限制（例如，HTTP 引用者，IP 地址，API 限制）。"}
  - 后端代理：{例如，"对于需要更多保密性或涉及敏感操作的密钥，必须创建后端代理端点；前端调用代理，而不是直接调用第三方服务。"}
- **安全通信（HTTPS）：**
  - 要求：{例如，"与后端 API 的所有通信必须使用 HTTPS。禁止混合内容（HTTPS 页面上的 HTTP 资源）。"}
- **依赖漏洞：**
  - 过程：{例如，"在 CI 中运行 `npm audit --audit-level=high`（或等效命令）。高/严重漏洞必须在部署前解决。监控 Dependabot/Snyk 警报。"}

## 浏览器支持和渐进增强

{本节定义了目标浏览器以及应用程序在功能较少或非标准环境中的行为。}

- **目标浏览器：** {例如，"Chrome、Firefox、Safari、Edge 的最新两个稳定版本。如果项目约束要求，可以列出特定版本。不支持 Internet Explorer（任何版本）。" 必须明确。}
- **Polyfill 策略：**
  - 机制：{例如，"在应用程序入口点导入 `core-js@3`。Babel `preset-env` 配置为包含上述浏览器目标所需的 polyfill。"}
  - 特定 Polyfill（如有，超出 `core-js`）：{列出特定功能所需的其他 polyfill，例如，`smoothscroll-polyfill`。}
- **JavaScript 要求和渐进增强：**
  - 基线：{例如，"核心应用程序功能要求浏览器启用 JavaScript。" 或 "关键内容（如文章、产品信息）和主要导航必须在没有 JavaScript 的情况下可访问和可读。交互功能和增强通过 JavaScript 分层添加（渐进增强方法）。" 指定所选方法。}
  - 无 JS 体验（如采用渐进增强）：{描述在没有 JS 时哪些功能可用。例如，"用户可以查看页面和导航。表单可能无法提交或使用标准 HTML 提交。"}
- **CSS 兼容性和回退：**
  - 工具：{例如，"使用 Autoprefixer（通过 PostCSS）配置目标浏览器列表以添加供应商前缀。"}
  - 功能使用：{例如，"避免使用目标浏览器支持率低于 90% 的 CSS 功能，除非明确定义并测试了优雅降级或回退（例如，使用 `@supports` 查询）。"}
- **可访问性回退：** {考虑在支持矩阵内，如果旧版辅助技术不支持某些 ARIA 版本或高级可访问性功能，这些功能的行为如何。}

## 变更日志

| 变更 | 日期 | 版本 | 描述 | 作者 |
| ---- | ---- | ---- | ---- | ---- |
