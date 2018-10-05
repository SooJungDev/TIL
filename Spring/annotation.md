## annotation 정리
@RequestBody   
*  Http 요청으로 body 내용을 자바 객체로 맵핑하는 역할
@ResponseBody
*  자바객체를 http 요청의 body 내용으로 맵핑하는 역할

@RestController
* Spring4.0 @Controller 와 @ResponseBody를 합쳐놓은 @RestController 추가
*  @ResponseBody 모든 요청 매핑 메소드에 추가할 필요가없음

@GetMapping
* @RequestMapping(Method=RequestMethod.GET)과 같음
* @PostMapping,@PutMapping,@PatchMapping,@DeteleMapping

@PathVariable
* 중괄호에 명시된 값을 변수로 받는다
* @RequestMapping(“/user/{id}”)
* Public String userInfo(@PatchVariable(“id) String id) 

@RequestParam
*  http 요청 파라미터를 변수로 받는다
* @RequestParam의 경우 url 뒤에 붙는 파라메터의 값을 가져올 때 사용
*  /test?param=1와 같이 url이 전달될 때 param 파라메터를 받아

## http메소드
* POST (create)
* GET(select)
* PUT(전체 update)
* PATCH(부분 update)
* DELETE(delete)



## 참고사이트
* @RestController http://highcode.tistory.com/24
* @RequestBody 와 @ResponseBody 개념 http://ithub.tistory.com/42
* Annotation 정리  http://jeong-pro.tistory.com/151
* Http 메소드 정리 https://m.blog.naver.com/azure0777/220824614635 
* @PathVariable http://homesi.tistory.com/entry/PathVariable-%EC%82%AC%EC%9A%A9%EB%B2%95-%EB%B0%8F-%ED%99%9C%EC%9A%A9%EB%B2%95