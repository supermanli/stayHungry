# 1、使用mvn打包源码并发布到本地仓库

​		

1、下载源代码并找到自己的源码位置，输入cmd 进入命令行，使用 mvn package将源码打包成jar包，

2、安装到本地仓库，执行以下命令（其中的-Dfile/-DgroupId/-DartifactId/-Dversion项根据pom文件内容填写）：

```xml
mvn install:install-file -Dfile=xxxxx.jar -DgroupId=xxx.xxx.xxx -DartifactId=xxxxx -Dversion=xxx.xxx.xxx -Dpackaging=jar
```

3、安装之后可以在本地仓库中找到对应的jar包。然后将对应的依赖信息插入到工程的最外面的pom文件即可：

```xml
<dependency>
    <groupId>xxx.xxx.xxx</groupId>
    <artifactId>xxxxx</artifactId>
    <version>xxx.xxx.xxx</version>
</dependency>
```

# 2、spring boot 项目中对密码进行加密与解密

## 使用说明

### 1.pom引入依赖

```xml
<dependency>
    <groupId>com.github.ulisesbocchio</groupId>
    <artifactId>jasypt-spring-boot-starter</artifactId>
    <version>2.1.1</version>
</dependency>
```

### 2.配置文件application.yaml

```yml
#******************加解密相关配置*******************
jasypt:
    encrytor:
        #用来加解密的salt值
        password: 123456
        #用来使用新的算法，默认为org.jasypt.salt.NoOPIVGenerator,这样的话我们就无法使用命令行中生成的密文，默认加密算法是			 PBEWithMD5AndDES`
        ivGeneratorClassname: org.jasypt.salt.RandomIVGenerator
```

参数解释：

- password:加密时候要使用salt值
- 对于ivGeneratorClassname，jasypt包中封装类默认为org.jasypt.salt.NoOpIVGenerator,这个时候我们如果使用Junit生成密文，那么只会生成24位密钥，与命令行中用命令生成的不一样，



加密：

```java
java -cp jasypt-1.9.2.jar org.jasypt.intf.cli.JasyptPBEStringEncryptionCLI input="123456" password=YOUAREAHACK algorithm=PBEWithMD5AndDES
```

解密： 

```java
java -cp jasypt-1.9.2.jar org.jasypt.intf.cli.JasyptPBEStringDecryptionCLI input="nCHNlWhifKG+ZF8r1A1fOA==" password=YOUAREAHACK algorithm=PBEWithMD5AndDES
```

指令里面的关键词：
password : 是自己自定义的密钥。
algorithm : 使用的加密算法。

# 3、MySQL Date函数

下面的表格列出了 MySQL 中最重要的内建日期函数：

| 函数                                                         | 描述                                |
| :----------------------------------------------------------- | :---------------------------------- |
| [NOW()](https://www.w3school.com.cn/sql/func_now.asp)        | 返回当前的日期和时间                |
| [CURDATE()](https://www.w3school.com.cn/sql/func_curdate.asp) | 返回当前的日期                      |
| [CURTIME()](https://www.w3school.com.cn/sql/func_curtime.asp) | 返回当前的时间                      |
| [DATE()](https://www.w3school.com.cn/sql/func_date.asp)      | 提取日期或日期/时间表达式的日期部分 |
| [EXTRACT()](https://www.w3school.com.cn/sql/func_extract.asp) | 返回日期/时间按的单独部分           |
| [DATE_ADD()](https://www.w3school.com.cn/sql/func_date_add.asp) | 给日期添加指定的时间间隔            |
| [DATE_SUB()](https://www.w3school.com.cn/sql/func_date_sub.asp) | 从日期减去指定的时间间隔            |
| [DATEDIFF()](https://www.w3school.com.cn/sql/func_datediff_mysql.asp) | 返回两个日期之间的天数              |
| [DATE_FORMAT()](https://www.w3school.com.cn/sql/func_date_format.asp) | 用不同的格式显示日期/时间           |

## 定义和用法

DATE_SUB() 函数从日期减去指定的时间间隔。

### 语法

```mysql
DATE_SUB(date,INTERVAL expr type)
```

```sql
SELECT OrderId,DATE_SUB(OrderDate,INTERVAL 2 DAY) AS OrderPayDate
FROM Orders
```

## 定义和用法

DATE_FORMAT() 函数用于以不同的格式显示日期/时间数据。

### 语法

```sql
DATE_FORMAT(date,format)
```

*date* 参数是合法的日期。*format* 规定日期/时间的输出格式。

可以使用的格式有：

| 格式 | 描述                                           |
| :--- | :--------------------------------------------- |
| %a   | 缩写星期名                                     |
| %b   | 缩写月名                                       |
| %c   | 月，数值                                       |
| %D   | 带有英文前缀的月中的天                         |
| %d   | 月的天，数值(00-31)                            |
| %e   | 月的天，数值(0-31)                             |
| %f   | 微秒                                           |
| %H   | 小时 (00-23)                                   |
| %h   | 小时 (01-12)                                   |
| %I   | 小时 (01-12)                                   |
| %i   | 分钟，数值(00-59)                              |
| %j   | 年的天 (001-366)                               |
| %k   | 小时 (0-23)                                    |
| %l   | 小时 (1-12)                                    |
| %M   | 月名                                           |
| %m   | 月，数值(00-12)                                |
| %p   | AM 或 PM                                       |
| %r   | 时间，12-小时（hh:mm:ss AM 或 PM）             |
| %S   | 秒(00-59)                                      |
| %s   | 秒(00-59)                                      |
| %T   | 时间, 24-小时 (hh:mm:ss)                       |
| %U   | 周 (00-53) 星期日是一周的第一天                |
| %u   | 周 (00-53) 星期一是一周的第一天                |
| %V   | 周 (01-53) 星期日是一周的第一天，与 %X 使用    |
| %v   | 周 (01-53) 星期一是一周的第一天，与 %x 使用    |
| %W   | 星期名                                         |
| %w   | 周的天 （0=星期日, 6=星期六）                  |
| %X   | 年，其中的星期日是周的第一天，4 位，与 %V 使用 |
| %x   | 年，其中的星期一是周的第一天，4 位，与 %v 使用 |
| %Y   | 年，4 位                                       |
| %y   | 年，2 位                                       |

## 实例

下面的脚本使用 DATE_FORMAT() 函数来显示不同的格式。我们使用 NOW() 来获得当前的日期/时间：

```sql
DATE_FORMAT(NOW(),'%b %d %Y %h:%i %p')
DATE_FORMAT(NOW(),'%m-%d-%Y')
DATE_FORMAT(NOW(),'%d %b %y')
DATE_FORMAT(NOW(),'%d %b %Y %T:%f')
```

结果类似：

```
Dec 29 2008 11:45 PM
12-29-2008
29 Dec 08
29 Dec 2008 16:25:46.635
```

## SQL Server Date 函数

下面的表格列出了 SQL Server 中最重要的内建日期函数：

| 函数                                                         | 描述                             |
| :----------------------------------------------------------- | :------------------------------- |
| [GETDATE()](https://www.w3school.com.cn/sql/func_getdate.asp) | 返回当前日期和时间               |
| [DATEPART()](https://www.w3school.com.cn/sql/func_datepart.asp) | 返回日期/时间的单独部分          |
| [DATEADD()](https://www.w3school.com.cn/sql/func_dateadd.asp) | 在日期中添加或减去指定的时间间隔 |
| [DATEDIFF()](https://www.w3school.com.cn/sql/func_datediff.asp) | 返回两个日期之间的时间           |
| [CONVERT()](https://www.w3school.com.cn/sql/func_convert.asp) | 用不同的格式显示日期/时间        |

## SQL Date 数据类型

MySQL 使用下列数据类型在数据库中存储日期或日期/时间值：

- DATE - 格式 YYYY-MM-DD
- DATETIME - 格式: YYYY-MM-DD HH:MM:SS
- TIMESTAMP - 格式: YYYY-MM-DD HH:MM:SS
- YEAR - 格式 YYYY 或 YY

SQL Server 使用下列数据类型在数据库中存储日期或日期/时间值：

- DATE - 格式 YYYY-MM-DD
- DATETIME - 格式: YYYY-MM-DD HH:MM:SS
- SMALLDATETIME - 格式: YYYY-MM-DD HH:MM:SS
- TIMESTAMP - 格式: 唯一的数字

# 4、文件输入输出流

```java
public void writeToFile(String data){
    byte[] sourceByte = data.getBytes();
    String path = "D:/file/";
    String fileName = "test.txt";
    if(null != sourceByte){
        try {
            File file = new File(path+fileName);//文件路径（路径+文件名）
            if (!file.exists()) {   //文件不存在则创建文件，先创建目录
                File dir = new File(file.getParent());
                dir.mkdirs();
                file.createNewFile();
            }
            FileOutputStream outStream = new FileOutputStream(file); //文件输出流将数据写入文件             	outStream.write(sourceByte);
            outStream.close();
        } catch (Exception e) {
            e.printStackTrace();
            // do something
        } finally {
            // do something
        }
    }
}
```

# 5、[通过JAVA对FTP服务器连接，上传，下载，读取，移动文件等](https://www.cnblogs.com/bigbaby/p/13279355.html)

## 记录一次对FTP服务器文件内容[#](https://www.cnblogs.com/bigbaby/p/13279355.html#2359297129)

> 通过Java程序对FTP服务器文件处理：连接，上传，下载，读取，移动文件等。
> 需求描述：今天接到一个任务，在Java项目中，读取FTP服务器上的一些文件，进行一些业务操作。处理完之后，将读取过的文件移动到其他目录里面。这就是我需求。

记录一下使用过程：这里使用的是 `org.apache.commons.net.ftp.FTP` 这个FTP处理包。

碰到的几个你可能看这篇文章要解决的碰到的问题，提前说下解决方案：

1. 完全不知道FTP的API如何使用，无从下手？（直接看方法就行了，很简单）
2. 访问服务器的时候连接成功，但是使用ListFile()获取不到任何文件？（就是第三个问题描述的）
3. 想要移动FTP服务器里的文件，如何办？（我使用了Rename操作来完成的。把原有的名字）
4. 具体业务实现。（你自己厘清自己的实现思路，无论是读取指定文件夹的内容，还是上传或者下载，亦或者是对文件名处理，split拼接，判断指定类型等等。都不是什么难的操作。按照自己的需求，自己去设计实现吧。）
5. 上传下载读取的话，都是使用流的操作。（看代码吧。）
6. 聊一下关于修改FTP工作空间的操作和设置数据传输模式。（这个先简单的提供两行代码，可能就已经能够解决你的疑问了）

```java
Copy//修改工作目录： 就是说，你要处理哪里的文件什么的。看一下Doc就知道了。
ftpClient.changeWorkingDirectory(pathname); 
//开启本地传输模式。（还有其他几种模式。Doc文档中有。自行查阅）
ftpClient.enterLocalPassiveMode();
```

我这里提炼出来了几个方法，新手使用的话，或者有问题的看代码自行参悟吧。比较简单，这里也主要就是对代码上的记录。（为了保护隐私，隐藏了一些隐私数据和业务处理类。）
关于代码，先说注意事项：

1. 用完连接，记得关。
2. 用完流，记得关。
3. 有异常，记得处理。

下面是具体的代码：

1. 建立FTP连接。

```java
Copypublic FTPClient ftpClient = null;
    //ftp服务器IP
    public String hostname = "XXX10.1001.11";
    //ftp服务器端口号默认为21
    public Integer port = XXX;
    //ftp登录账号
    public String username = "XXX";
    //ftp登录密码
    public String password = "XXX";

    public void initFtpClient() {
        ftpClient = new FTPClient();
        ftpClient.setControlEncoding("utf-8");
        try {
            System.out.println("connecting...ftp服务器:"+hostname+":"+port);
            ftpClient.connect(hostname, port); //连接ftp服务器
            ftpClient.login(username, password); //登录ftp服务器
            int replyCode = ftpClient.getReplyCode(); //是否成功登录服务器
            if(!FTPReply.isPositiveCompletion(replyCode)){
                System.out.println("connect failed...ftp服务器:"+hostname+":"+port);
            }
            System.out.println("connect successfu...ftp服务器:"+hostname+":"+port);
        }catch (MalformedURLException e) {
            e.printStackTrace();
        }catch (IOException e) {
            e.printStackTrace();
        }
    }
```

调用完 intFTPClient（）方法之后，就可以使用ftpClient 来操作FTP服务器了。

1. 上传.

```java
Copy     /**
     * 上传文件
     * @param pathname ftp服务保存地址
     * @param fileName 上传到ftp的文件名
     *  @param originfilename 待上传文件的名称（绝对地址） *
     * @return*
     **/

    public boolean uploadFile( String pathname, String fileName,String originfilename){
        boolean flag = false;
        InputStream inputStream = null;
        try{
            System.out.println("开始上传文件");
            inputStream = new FileInputStream(new File(originfilename));
            initFtpClient();

            ftpClient.setFileType(ftpClient.BINARY_FILE_TYPE);
            CreateDirecroty(pathname);
            ftpClient.makeDirectory(pathname);
            ftpClient.changeWorkingDirectory(pathname);
            ftpClient.enterLocalPassiveMode();
            ftpClient.storeFile(fileName, inputStream);
            inputStream.close();
            ftpClient.logout();
            flag = true;
            System.out.println("上传文件成功");
        }catch (Exception e) {
            System.out.println("上传文件失败");
            e.printStackTrace();
            return false;
        }finally{
            if(ftpClient.isConnected()){
                try{
                    ftpClient.disconnect();
                }catch(IOException e){
                    e.printStackTrace();
                    flag = false;
                }
            }
            if(null != inputStream){
                try {
                    inputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                    flag = false;
                }
            }
        }
        return flag;
    }
```

1. 下载

```java
Copy//我这里在读到文件流的时候，没有保存到本地，直接在程序里面读取转换里面的内容，继续使用了。
//如果有需要的话，我简单的说下思路。就是 用 ftpClient调用指定文件下的文件，调用 FTPFile[] ftpFileArr = ftpClient.listFiles(path);
//后续 InputStream in = ftpClient.retrieveFileStream(fileName)。 获取到具体的文件流。然后后续操作 保存流到本地即可。
```

1. 移动文件（通过Rename完成）

```java
Copy     /**
     *
     * @param nameSet 需要移动的文件目录清单
     * @param fromPath 从哪个目录移动
     * @param toPath 要移动的目录（从根目录开始写。如：/code/yes/:根目录下的code目录下的yes目录
     * @return 返回成功或者失败
     * 注意：目录的话，以/结尾
     */
    public boolean moveFile(List<String> nameSet,String fromPath,String toPath){

        //name格式：/code/yesdawaTest1.txt 、/code/yesdawaTest2.txt
        if (nameSet != null && nameSet.size() > 0) {
            for (String s : nameSet) {
                try {
                    //获取连接：因为一次连接改多个文件只能有一个生效。这里每次修改文件的时候，获取一次新的链接
                    initFtpClient();
                    //改变工作目录
                    ftpClient.changeWorkingDirectory(fromPath);
                    ftpClient.enterLocalPassiveMode();
                    ftpClient.setFileType(FTP.BINARY_FILE_TYPE);

                    String[] split = s.split("/");
                    String name = "";
                    if (split.length > 0) {
                        name = split[split.length - 1];
                    }
                    boolean rename = ftpClient.rename(name, toPath + name);
                    if (rename) {
                        System.out.println(name + "成功移动到:" + toPath + name);
                    }
                    ftpClient.logout();
                } catch (IOException e) {
                    e.printStackTrace();
                    return false;
                }
            }
        }
        return true;
    }
```

1. 对文件名或文件类型的处理和判断。（仅供参考）

```java
Copy     //检查指定/path目录下，是否有.txt结尾的文件。如果有，返回符合要求的.
   /**
    *
    * @param path:要查找的文件目录，如 /code：根目录下的code目录
    * @param ext:要查找的文件类型扩展名，如 .txt
    * @return 返回：符合查找要求的 文件名清单。
    *
    * 返回的 文件名清单：可以先用于处理数据，然后再处理数据成功后，移动这些文件到其他目录下。
    */
   public List<String> checkExistFile(String path, String ext){

       //定义一个返回集合：
       List<String> tNameSet = new ArrayList<>();
       //连接服务器
       initFtpClient();
       try {
           //改变工作目录
           ftpClient.changeWorkingDirectory(path);
           ftpClient.enterLocalPassiveMode();
           ftpClient.setFileType(FTP.BINARY_FILE_TYPE);

           String[] strings = ftpClient.listNames(path);
           if (strings.length > 0) {
               for (String string : strings) {
                   //如果文件名的长度（带后缀名的）>4 . 才可能是 .txt 结尾的。
                   if (string.length() > ext.length()) {
                       //取得文件后缀名
                       String substring = string.substring(string.length() - ext.length());
                       if (ext.equals(substring)) {
                           System.out.println("目前正在处理文件：" + string);
                           tNameSet.add(string);
                       }
                   }
               }
           }
           ftpClient.logout();
       } catch (IOException e) {
           System.out.println("读取:"+path+" 目录失败");
           e.printStackTrace();
       }

       //返回：符合要求的文件 名字 清单
       return tNameSet;
   }
```

1. 关闭ftp连接

```java
Copy     /**
     * 关闭FTP服务链接
     * @throws IOException
     */
    public static void disConnection(FTPClient ftpClient){
        try {
            if(ftpClient.isConnected()){
                ftpClient.disconnect();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
```

OK，以上就是所有问题，有问题可以发邮件联系：16637462812@163.com。 欢迎沟通交流。

作者： dawa大娃bigbaby

出处：https://www.cnblogs.com/bigbaby/p/13279355.html

版权：本文采用「[署名-非商业性使用-相同方式共享 4.0 国际](https://creativecommons.org/licenses/by-nc-sa/4.0/)」知识共享许可协议进行许可。

### 样例

```java
package com.chinaunicom.cs.gcp.provider.scheduledtask;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.util.Properties;

import com.jcraft.jsch.Channel;
import com.jcraft.jsch.ChannelSftp;
import com.jcraft.jsch.JSch;
import com.jcraft.jsch.Session;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;

public class SFTPUtil {
    static Session sshSession = null;
    /**
     * 获取ChannelSftp
     *
     * @param host
     *            主机
     * @param sOnlineSftpPort
     *            端口
     * @param username
     *            用户名
     * @param password
     *            密码
     * @return
     */
    public static ChannelSftp getConnectIP(String host, String sOnlineSftpPort, String username, String password) {
        int port = 0;
        if (!("".equals(sOnlineSftpPort)) && null != sOnlineSftpPort) {
            port = Integer.parseInt(sOnlineSftpPort);
        }
        ChannelSftp sftp = null;
        try {
            JSch jsch = new JSch();
            jsch.getSession(username, host, port);
            sshSession = jsch.getSession(username, host, port);
            sshSession.setPassword(password);
            Properties sshConfig = new Properties();
            sshConfig.put("StrictHostKeyChecking", "no");
            sshSession.setConfig(sshConfig);
            sshSession.connect();
            Channel channel = sshSession.openChannel("sftp");
            channel.connect();
            sftp = (ChannelSftp) channel;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return sftp;
    }

    /**
     * 上传
     *
     * @param directory
     *            sftp 服务器目录
     * @param uploadFile
     *            上传文件路径
     * @param sftp
     */
    public static void upload(String directory, File uploadFile, ChannelSftp sftp) {
        try {
            sftp.cd(directory);
            sftp.put(new FileInputStream(uploadFile), uploadFile.getName());
            uploadFile.delete();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (sftp.isConnected()) {
                sshSession.disconnect();
                sftp.disconnect();
            }

        }
    }


    /**
     * 下载
     *
     * @param directory
     *            sftp服务器目录
     * @param downloadFile
     *            目录下的文件名称
     * @param saveFile
     *            本地保存文件路径
     * @param sftp
     */
    public static void download(String directory, String downloadFile, String saveFile, ChannelSftp sftp) {
        try {
            sftp.cd(directory);
            File file = new File(saveFile);
            sftp.get(downloadFile, new FileOutputStream(file));
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (sftp.isConnected()) {
                sshSession.disconnect();
                sftp.disconnect();
            }

        }
    }

    /**
     * 删除
     *
     * @param directory
     * @param deleteFile
     * @param sftp
     */
    public static void delete(String directory, String deleteFile, ChannelSftp sftp) {
        try {
            sftp.cd(directory);
            sftp.rm(deleteFile);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (sftp.isConnected()) {
                sshSession.disconnect();
                sftp.disconnect();
            }

        }
    }
}
```
