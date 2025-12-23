## 개요
Next.js (App Router)에 익숙한 상태에서 사전 과제를 위해 React Router를 사용하다 보니, `BrowserRouter`와 v6.4 이후 등장한 `createBrowserRouter`의 차이점이 기억나지 않아서 작성한 문서이다.

본 문서는 **현재 React Router가 지향하는 라우팅 방식(Data Router)**이 무엇인지, 그리고 기존 방식과 어떤 구조적 차이가 있는지 공식 문서를 기반으로 정리하였다.

> React Router v7.11 기준으로 작성되었다.

## BrowserRouter
`BrowserRouter`는 v6.4 이전의 전통적인 방식으로 HTML5 History API를 사용하여 클라이언트 사이드 라우팅을 제공하는 **가장 기본적인 Router 구현체**이다.

```tsx
<BrowserRouter>
    <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/users/:id" element={<User />} />
    </Routes>
</BrowserRouter>
```

### 특징
* **UI 중심 라우팅**: URL과 화면(Component)을 연결하는 역할에 집중한다.
* **Data API 사용 불가**: `loader`, `action`, `fetcher` 등의 최신 기능을 사용할 수 없다.
* **Fetch-on-Render (Waterfalls)**: 컴포넌트가 마운트된 후(`useEffect`) 데이터를 요청하므로, 중첩된 라우트에서 데이터 로딩이 순차적으로 발생하여 느려질 수 있다.

### 성격
* React 컴포넌트 트리 내부에서 동작하며 구조적 제약이 적다.
* 데이터 로딩과 에러 처리는 각 컴포넌트가 개별적으로 책임진다.

## createBrowserRouter

`createBrowserRouter`는 v6.4부터 도입된 **Data Router**를 생성하는 API이다.  
라우팅을 단순한 화면 전환이 아닌 **데이터와 UI의 결합**으로 관리한다.

```tsx
const router = createBrowserRouter([
    {
        path: '/',
        element: <Root />,
        loader: rootLoader, // 데이터 로딩
        errorElement: <ErrorPage />, // 에러 처리
        children: [
            {
                index: true,
                element: <Index />,
                loader: indexLoader,
            },
        ],
    },
]);

<RouterProvider router={router} />
```

### 특징
* **설정 객체 기반 정의**: 라우트 구조를 객체 배열(또는 `createRoutesFromElements`)로 정의한다.
* **Render-as-you-fetch (병렬 로딩)**: URL 변경 즉시 라우트와 데이터를 병렬로 로드하여 렌더링 속도를 최적화한다.
* **라우트 레벨 상태 관리**: `loader`(데이터 읽기), `action`(데이터 쓰기), `errorElement`(에러)를 라우트 설정에서 직접 관리한다.

### 성격
* Router가 애플리케이션의 상태 전이(Navigation)와 데이터 흐름을 주도하는 역할을 한다.
* SSR(Server-Side Rendering) 및 Hydration 시나리오와 호환된다.

## 구조적 차이 비교

| 구분 | BrowserRouter | createBrowserRouter |
| --- | --- | --- |
| **라우트 정의** | JSX (`<Routes>`) | 객체 배열 (권장) 또는 JSX 헬퍼 |
| **Data API** | **불가능** | **가능** (loader, action, fetcher) |
| **라우팅 책임** | URL ↔ UI | URL ↔ 데이터 ↔ UI |
| **데이터 로딩 시점** | 컴포넌트 렌더링 후 (Waterfall) | **렌더링 전 또는 병렬 수행** (Parallel) |
| **에러 처리** | 개별 컴포넌트 내부 (`try-catch`) | `errorElement` (Bubble up 방식) |
| **v7 기준 포지션** | 레거시 호환 및 단순 앱용 | **표준 (권장)** |

## Next.js App Router 유저를 위한 비유

Next.js App Router의 개념을 React Router(`createBrowserRouter`)에 대입하면 다음과 같이 이해할 수 있다.

| Next.js (App Router) | React Router (createBrowserRouter) | 역할 |
| --- | --- | --- |
| **Server Component Fetching** | **`loader`** | 페이지 진입 전 데이터 미리 로드 |
| **Server Actions** | **`action`** | Form 제출 및 데이터 Mutation 처리 |
| **`error.tsx`** | **`errorElement`** | 해당 라우트 세그먼트의 에러 캡처 UI |
| **`layout.tsx`** | **Parent Route + `<Outlet />`** | 중첩 레이아웃 구조 |

## 정리
* **BrowserRouter**는 단순히 **URL에 따라 컴포넌트를 보여주는 것**이 목표일 때 사용한다.
* **createBrowserRouter**는 **URL 이동을 데이터 로딩 및 상태 변화의 트리거**로 보고, 사용자 경험(UX)을 최적화할 때 사용한다.
* **결론**: 최신 React 생태계에서는 `createBrowserRouter`를 사용하여 라우트 단위로 책임을 분리하는 것이 표준이다.