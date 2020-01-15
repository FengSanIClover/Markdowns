# Insecure Transport: Mail Transmission

### RiskLevel : Critical

![Insecure_Transport_Mail_Transmission_1](/Fortify/Critical/Insecure_Transport_Mail_Transmission/Insecure_Transport_Mail_Transmission_1.png "Insecure_Transport_Mail_Transmission")

## Tips
透過程式**寄送電子郵件，未使用加密的連線**，將有可能被攔截網路流量的攻擊者讀取或修改。

## 程式碼範例

### 修改前

``` C#
public int SendMail(string from, List<string> to, string Subject, string BodyContext, string Encoding = "Default")
{
    try
    {
        MailMessage mail = new MailMessage();
        mail.From = new MailAddress(from);
        if (to.Any())
        {
            foreach (var address in to)
            {
                if (!string.IsNullOrEmpty(address))
                {
                    mail.To.Add(address);
                }
            }
        }

        mail.Subject = Subject;
        mail.Body = BodyContext;

        switch (Encoding)
        {
            case "Default":
                mail.BodyEncoding = System.Text.Encoding.Default;
                mail.HeadersEncoding = System.Text.Encoding.Default;
                break;
            case "BIG5":
                mail.BodyEncoding = System.Text.Encoding.GetEncoding(950);
                mail.HeadersEncoding = System.Text.Encoding.GetEncoding(950);
                break;
            case "UTF8":
                mail.BodyEncoding = System.Text.Encoding.UTF8;
                mail.HeadersEncoding = System.Text.Encoding.UTF8;
                break;
            case "UTF7":
                break;
            default:
                goto case "Default";
        }

        Configuration config = WebConfigurationManager.OpenWebConfiguration(HttpContext.Current.Request.ApplicationPath);

        MailSettingsSectionGroup settings =
            (MailSettingsSectionGroup)config.GetSectionGroup("system.net/mailSettings");

        SmtpClient smtp = new SmtpClient();
        smtp.Send(mail);

        return 0;
    }
    catch (Exception ex)
    {
        ExceptionData = ex;
        LogHelper.SystemLogger.Info($"ExceptionData:{ex.ToString()}", SCB.EStamp.Domain.Enums.SystemLogEvent.ES0002, SCB.EStamp.Domain.Enums.SystemLogType.Other, "SendMail");
        return 9999;
    }
}
```

>加上： smtp.EnableSsl = true; 使用 SSL 加密，但要確認使用的 Email Server 是否有支援 SSL。

### 修改後

```C#
public int SendMail(string from, List<string> to, string Subject, string BodyContext, string Encoding = "Default")
{
    try
    {
        MailMessage mail = new MailMessage();
        mail.From = new MailAddress(from);
        if (to.Any())
        {
            foreach (var address in to)
            {
                if (!string.IsNullOrEmpty(address))
                {
                    mail.To.Add(address);
                }
            }
        }

        mail.Subject = Subject;
        mail.Body = BodyContext;

        switch (Encoding)
        {
            case "Default":
                mail.BodyEncoding = System.Text.Encoding.Default;
                mail.HeadersEncoding = System.Text.Encoding.Default;
                break;
            case "BIG5":
                mail.BodyEncoding = System.Text.Encoding.GetEncoding(950);
                mail.HeadersEncoding = System.Text.Encoding.GetEncoding(950);
                break;
            case "UTF8":
                mail.BodyEncoding = System.Text.Encoding.UTF8;
                mail.HeadersEncoding = System.Text.Encoding.UTF8;
                break;
            case "UTF7":
                break;
            default:
                goto case "Default";
        }

        Configuration config = WebConfigurationManager.OpenWebConfiguration(HttpContext.Current.Request.ApplicationPath);

        MailSettingsSectionGroup settings =
            (MailSettingsSectionGroup)config.GetSectionGroup("system.net/mailSettings");

        SmtpClient smtp = new SmtpClient();
        smtp.EnableSsl = true;
        smtp.Send(mail);

        return 0;
    }
    catch (Exception ex)
    {
        ExceptionData = ex;
        LogHelper.SystemLogger.Info($"ExceptionData:{ex.ToString()}", SCB.EStamp.Domain.Enums.SystemLogEvent.ES0002, SCB.EStamp.Domain.Enums.SystemLogType.Other, "SendMail");
        return 9999;
    }
}
```