# 프로젝트 정보

- youtube api를 이용해 YouTube 사이트 클론

## 구현 사항

### Mock Data를 이용해 개발하는 방법.

- class를 이용해 constructor로 baseURL url 연결
- ApiContext 파일에서 실제 data와 Mock data의 인스턴스를 필요에 맞게 사용

```javascript
import { createContext, useContext } from "react";
import FakeYoutube from "../api/fakeYoutube";
import Youtube from "../api/youtube";

export const YoutubeApiContext = createContext();

const youtube = new Youtube();
// const youtube = new FakeYoutube();

export function YoutubeApiProvider({ children }) {
  return (
    <YoutubeApiContext.Provider value={{ youtube }}>
      {children}
    </YoutubeApiContext.Provider>
  );
}

export function useYoutubeApi() {
  return useContext(YoutubeApiContext);
}
```

### useEffect hook을 이용한 검색 기능 구현.

- useParams()를 이용해서 url의 keyword 가져오기

```javascript
useEffect(() => {
  setText(keyword || "");
}, [keyword]);
```

### Tanstack query로 네트워크 통신.

- data fetching을 위해 staleTime 설정

```javascript
const {
  isLoading,
  error,
  data: videos,
} = useQuery(["videos", keyword], () => youtube.search(keyword), {
  staleTime: 1000 * 60 * 1,
});
```

### Tailwind CSS 세팅 방법

- 사용자 지정 className 사용하기 위해 extend에 원하는 속성 지정하기

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx}"],
  theme: {
    extend: {
      colors: {
        brand: "#ff0000",
      },
    },
  },
  plugins: [require("@tailwindcss/line-clamp")],
};
```

### 상세페이지

- iframe 태그를 이용한 player 설정

```javascript
<iframe
  id="player"
  type="text/html"
  width="100%"
  height="640"
  src={`https://www.youtube.com/embed/${video.id}`}
  frameBorder="0"
  title={title}
/>
```
