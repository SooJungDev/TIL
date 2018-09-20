## vue router history mode 설정

~~~javascript
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
~~~

- 라우터 기본 모드는 hash mode URL 해시를 사용하여 전체 URL을 시뮬레이트하므로 URL이 변경될 때 페이지가 다시 로드 되지 않음
- 해시를 제거하기 위해 라우터의 history 모드 를 사용
- 서버 설정이 없는 단일 페이지 클라이언트 앱이기 때문에 사용자가 직접 http://oursite.com/user/id 에 접속하면 404 오류가 발생
- 서버에 간단하게 포괄적인 대체 경로를 추가하기만 하면됩니다. URL이 정적 에셋과 일치하지 않으면 앱이 있는 동일한 index.html페이지를 제공

## 참고사이트
- https://router.vuejs.org/kr/guide/essentials/history-mode.html#%EC%84%9C%EB%B2%84-%EC%84%A4%EC%A0%95-%EC%98%88%EC%A0%9C
