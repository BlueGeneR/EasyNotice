# EasyNotice

## Nuget��

| Package Name |  Version | Downloads
|--------------|  ------- | ----
| EasyNotice.Core | ![](https://img.shields.io/badge/nuget-v1.2.0-blue) | ![](https://img.shields.io/badge/downloads-xM-brightgreen)|
| EasyNotice.Dingtalk | ![](https://img.shields.io/badge/nuget-v1.2.0-blue) | ![](https://img.shields.io/badge/downloads-xM-brightgreen)|
| EasyNotice.Email | ![](https://img.shields.io/badge/nuget-v1.2.0-blue) | ![](https://img.shields.io/badge/downloads-xM-brightgreen)|

---------

# `EasyNotice`
> ����һ������.NET��Դ����Ϣ֪ͨ��������������ʼ�֪ͨ������֪ͨ�����԰������Ǹ����׵ط��ͳ����쳣֪ͨ��

-------

## ���ܽ���
 - ֧���ʼ����͡���������
 - ֧���Զ��巢�ͼ��������ͬ�����쳣Ƶ��֪ͨ
 - ɵ��ʽ���ã����伴��


 ---------

# ��Ŀ����

## 1. �����ʼ�֪ͨ
> �ʼ�֪֧ͨ��ͬʱ���͸�����ռ���
### Step 1 : ��װ����ͨ��Nuget��װ��

```powershell
Install-Package EasyNotice.Core
Install-Package EasyNotice.Email
```

### Step 2 : ���� Startup ������

```csharp
public class Startup
{
    //...
    
    public void ConfigureServices(IServiceCollection services)
    {
        //configuration
        services.AddEsayNotice(config =>
        {
            config.IntervalSeconds = 10;//ͬһ�������Ϣ��10����ֻ�ܷ�һ���������ʱ���ڴ��������ظ���Ϣ
            config.UseEmail(option =>
            {
                option.Host = "smtp.qq.com";//smtp����
                option.Port = 465;//�˿�
                option.FromName = "System";//���������֣��Զ��壩
                option.FromAddress = "12345@qq.com";//��������
                option.Password = "passaword";//��Կ
                option.ToAddress = new List<string>()//�ռ��˼���
                {
                    "12345@qq.com"
                };
            });
        });
    }    
}
```

### Step 3 : IEmailProvider����ӿ�ʹ��

```csharp
[ApiController]
[Route("[controller]/[action]")]
public class NoticeController : ControllerBase
{
    private readonly IEmailProvider _mailProvider;
    public NoticeController(IEmailProvider provider)
    {
        _mailProvider = provider;
    }

    [HttpGet]
    public async Task SendMail([FromQuery] string str)
    {
        //�����ʼ�
        await _mailProvider.SendAsync(str, new Exception(str));
    }
}
```
---------


## 2. ������֪ͨ
### Step 1 : ��װ����ͨ��Nuget��װ��

```powershell
Install-Package EasyNotice.Core
Install-Package EasyNotice.Dingtalk
```

### Step 2 : ���� Startup ������

```csharp
public class Startup
{
    //...
    
    public void ConfigureServices(IServiceCollection services)
    {
        //configuration
        services.AddEsayNotice(config =>
        {
            config.IntervalSeconds = 10;//ͬһ�������Ϣ��10����ֻ�ܷ�һ���������ʱ���ڴ��������ظ���Ϣ
            config.UseDingTalk(option =>
            {
                option.WebHook = "https://oapi.dingtalk.com/robot/send?access_token=xxx";
                option.Secret = "secret";
            });
        });
    }    
}
```

### Step 3 : IDingtalkProvider����ӿ�ʹ��

```csharp
[ApiController]
[Route("[controller]/[action]")]
public class NoticeController : ControllerBase
{
    private readonly IDingtalkProvider _dingtalkProvider;
    public NoticeController(IDingtalkProvider dingtalkProvider)
    {
        _dingtalkProvider = dingtalkProvider;
    }

    [HttpGet]
    public async Task SendDingTalk([FromQuery] string str)
    {
        await _dingtalkProvider.SendAsync(str, new Exception(str));
    }
}
```

---------
## ����ʾ��

1. �鿴 [����ʹ������](https://github.com/Bryan-Cyf/EasyNotice/tree/master/sample)
2. �鿴 [�����������](https://github.com/Bryan-Cyf/EasyNotice/tree/master/test)

