<center><h1>C# 邮件 收发</h1></center>

- 使用IMAP接收邮件

  - 添加引用

  - ```C#
    using LumiSoft.net.Imap.client;
    using lumisoft.net.imap;
    using lumisoft.net.mail;
    ```

  - 正文

  - ```C#
    private string IMAP_URL="imap.exmail.qq.com";
    private int IMAP_PORT = 993;
    IMAP_Client client=new IMAP_Client();
    try
    {
        //与IMAP服务器建立连接
        client.connect(IMAP_URL,IMAP_PORT,false);
        //验证身份
        client.login(mail_user,mail_pasword);
        client.selectfolder("inbox");
        imap_client_fetchhandler fetchhandler=new imap_client_fetchhandler();
        //获取未读邮件的UID
        int[] unseen_ids=client.search(false,"UTF-8","unseen");
        imap_t_seqset sequence=imap_t_seqset.parse(string.join(",",unseen_ids));
        var items=new imap_t_fetch_i[]
        {
            new imap_t_fetch_i_envelope(),
            new imap_t_fetch_i_uid(),
            new imap_t_fetch_i_flags(),
            new imap_t_fetch_i_internalDate(),
            new imap_t_fetch_i_rfc822()
        }
        //遍历未读邮件
        client.fetch(false,seqence,items,(e,e)=>
        {
        	try
            {
                var email=e.Value as imap_r_u_fetch;
                if(email.rfc822!=null)
                {
                    email.rfc822.stream.position=0;
                    var mie=mail_message.parseFromStream(email.rfc822.stream);
                    email.rfc822.stream.close();
                    //存入数据库
                    //SaveEmail(mime.MainEntity.Subject, mime.MainEntity.Date, mime.BodyText, mime.BodyHtml,mime.MainEntity.From.ToAddressListString(), mime.MainEntity.To.ToAddressListString(),mime.MainEntity.Cc == null ? string.Empty : mime.MainEntity.Cc.ToAddressListString(),mime.MainEntity.Bcc == null ? string.Empty : mime.MainEntity.Bcc.ToAddressListString(),Sql.ToInteger(info.UID));
                    
                }
            }
            catch(Exception ex)
            {
                iblerror.text="handle-err:"+ex.message;
            }
        });
        
    }
    catch(exception ex)
    {
        iblerror.text=ex.message;
    }
    ```

  - | 服务器名称 | 服务器地址   | SSL协议端口号 | 非SSL协议端口号 |
    | ---------- | ------------ | ------------- | --------------- |
    | IMAP       | imap.163.com | 993           | 143             |
    | SMTP       | smtp.168.com | 465/994       | 25              |
    | POP3       | pop.163.com  | 995           | 110             |

- 使用POP3

  - ```C#
    using (POP3_Client pop3 = new POP3_Client())
                {
                    //与Pop3服务器建立连接
                    pop3.Connect(sSERVER_URL, nPORT, false);
                    //验证身份
                    pop3.Authenticate(sEMAIL_USER, sEMAIL_PASSWORD, false);
                    //获取邮件信息列表
                    POP3_ClientMessageCollection infos = pop3.Messages;
    
                    foreach (POP3_ClientMessage info in infos)
                    {
                        byte[] bytes = info.MessageToByte();
                        Mime mime = Mime.Parse(bytes);
                       //存库
                        SaveEmail(mime.MainEntity.Subject, mime.MainEntity.Date, mime.BodyText, mime.BodyHtml,mime.MainEntity.From.ToAddressListString(), mime.MainEntity.To.ToAddressListString(),mime.MainEntity.Cc == null ? string.Empty : mime.MainEntity.Cc.ToAddressListString(),mime.MainEntity.Bcc == null ? string.Empty : mime.MainEntity.Bcc.ToAddressListString(),Sql.ToInteger(info.UID));
                    }
                }
    ```

  [参考](<https://www.cnblogs.com/kangjing/p/6265043.html>)

