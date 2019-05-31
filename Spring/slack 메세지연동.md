## 슬랙 메세지 연동
1. 슬랙에서 채널 생성
2. 원하는 채널을 선택하고 접근 API 를 생성
- https://my.slack.com/services/new/incoming-webhook/
    - 채널 선택 한후에 Add Incoming WebHooks integration 선택 
    
3. Slack Message 만들기

- Web Hook 연결
~~~java
@Getter
public enum SlactTargetEnum {

    CH_BOT("발급받은 web hook url", "채널 이름");
    
    private final String webHookUrl;
    private final String channel;
    
    SlactTargetEnum(String webHookUrl, String channel){
        this.webHookUrl= webHookUrl;
        this.channel= channel;
    }
}
~~~

- Attachments JSON DTO
~~~java
@Getter
@NoArgsConstructor
public static class Attachments {
    private List<Attachment> attachments;
    @Builder
    public Attachments(List<Attachment> attachments) {
        this.attachments = attachments;
    }
}
@Getter
@NoArgsConstructor
public static class Attachment {
    private String fallback;
    private String color;
    private String pretext;
    private String author_name;
    private String author_link;
    private String author_icon;
    private String title;
    private String title_link;
    private String text;
    private String image_url;
    private String thumb_url;
    private String footer;
    private String footer_icon;
    private Long ts;
    private List<Field> fields;
    @Builder
    public Attachment(String fallback, String color, String pretext, String author_name, String author_link, String author_icon, String title, String title_link, String text, String image_url, String thumb_url, String footer, String footer_icon, Long ts, List<Field> fields) {
        this.fallback = fallback;
        this.color = color;
        this.pretext = pretext;
        this.author_name = author_name;
        this.author_link = author_link;
        this.author_icon = author_icon;
        this.title = title;
        this.title_link = title_link;
        this.text = text;
        this.image_url = image_url;
        this.thumb_url = thumb_url;
        this.footer = footer;
        this.footer_icon = footer_icon;
        this.ts = ts;
        this.fields = fields;
    }
}
~~~

- controller 
~~~
@RequestMapping(value = "attachment", method = POST)
public ResponseEntity attachment(@RequestBody SlackMessageDto.Attachments dto) {
    return ResponseEntity.ok(slackSenderManager.send(SlackTargetEnum.CH_BOT, dto));
}
~~~
 
- slack web hook 보내기
    - restTemplate 을 이용해서 해당 url 에  보내준다.
~~~java
public boolean send(SlackTargetEnum target, Object object) {
    try {
        restTemplate.postForEntity(target.getWebHookUrl(), writeValueAsString(object), String.class);
        return true;
    } catch (Exception e) {
        log.error("Occur Exception: {}", e);
        return false;
    }
}

~~~
 


## 참고사이트
- [Java Spring + Slack Web Hook API](https://printhelloworld.tistory.com/165)
- [Spring 으로 초간단 Slack Message 보내기](https://cheese10yun.github.io/slack-bot-spring/)
- [Slack API Message Formatting](https://api.slack.com/docs/messages/builder?msg=%7B%22text%22%3A%22I%20am%20a%20test%20message%20http%3A%2F%2Fslack.com%22%2C%22attachments%22%3A%5B%7B%22text%22%3A%22%ED%85%8C%EC%8A%A4%ED%8A%B8%22%7D%5D%7D)