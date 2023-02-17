# SpringBoot

## SpringBoot整合邮件服务

### 配置

登录到QQ邮箱：https://mail.qq.com/ 

选择账户

![image-20221206143045740](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221206143045740.png)

点击开启SMTP服务：

![image-20221206143209397](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221206143209397.png)

发送短信：

![image.png](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/1669891701970-d88a0897-e5ef-4aa8-8b64-33a9ddb879ec.png)

发送完，点击我已发送，然后得到密码：

![image-20221206143525274](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221206143525274.png)

### POM依赖：

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

### application.yml

```yaml
spring:
  mail:
    # 配置 SMTP 服务器地址 
    host: smtp.qq.com
    # 发送者邮箱
    username: 你的邮箱
    # 配置密码，注意不是真正的密码，而是刚刚申请到的授权码
    password: 申请的密码
    # 端口号465或587
    port: 587 
    # 默认的邮件编码为UTF-8
    default-encoding: UTF-8
```

### Java集成EmailService

```java
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.mail.javamail.MimeMessageHelper;
import org.springframework.stereotype.Component;

import javax.mail.MessagingException;
import javax.mail.internet.MimeMessage;
import java.util.Date;

@Component
@Slf4j
public class EmailUtils {
    @Autowired
    JavaMailSender javaMailSender;

    @Value("${spring.mail.username}")
    String username;

    public void sendHtml(String title, String html, String to) {
        MimeMessage mailMessage = javaMailSender.createMimeMessage();
        //需要借助Helper类
        MimeMessageHelper helper = new MimeMessageHelper(mailMessage);
        try {
            helper.setFrom(username);  // 必填
            helper.setTo(to);   // 必填
//            helper.setBcc("密送人");   // 选填
            helper.setSubject(title);  // 必填
            helper.setSentDate(new Date());//发送时间
            helper.setText(html, true);   // 必填  第一个参数要发送的内容，第二个参数是不是Html格式。
            javaMailSender.send(mailMessage);
        } catch (MessagingException e) {
            log.error("发送邮件失败", e);
        }
    }

}
```

### 在controller里定义接口：

```Java
@ApiOperation(value = "邮箱验证接口")
@GetMapping("/email")
public Result sendEmail(@RequestParam String email, @RequestParam String type) {
    userService.sendEmail(email, type);
    return Result.success();
}
```

### 在业务实现层UserServiceImpl写业务逻辑

```java
void sendEmail(String email, String type);

private static final Map<String, Long> CODE_MAP = new ConcurrentHashMap<String, Long>();

@Resource
EmailUtils emailUtils;
@Override
public void sendEmail(String email, String type) {
  String  code = RandomUtil.randomNumbers(6);
  log.info("本次验证码的code是：{}", code);
  String context = "<b>尊敬的用户：</b><br><br><br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;您好，" +
    "Partner交友网提醒您本次的验证码是：<b>{}</b>，" +
    "有效期5分钟。<br><br><br><b>Partner交友网</b>";
  String html = StrUtil.format(context, code);
  if ("REGISTER".equals(type)) {
    // 多线程异步请求
    ThreadUtil.execAsync(() -> {
      emailUtils.sendHtml("【partner交友网】邮箱注册验证",html, email);
    });
    CODE_MAP.put(code, System.currentTimeMillis());
  }
}
```

输入邮箱，点击发送：

![image-20221206152522658](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221206152522658.png)

在后端我们可以看到验证码为：

![image-20221206152612701](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221206152612701.png)

登录邮箱：查看邮件即可

![image-20221212103226949](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221212103226949.png)

