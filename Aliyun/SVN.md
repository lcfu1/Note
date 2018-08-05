# SVN教程

目录：

- [安装SVN](#安装svn)
- [建立SVN仓库](#建立svn仓库)
- [配置和管理SVN](#配置和管理svn)
- [启动和停止SVN](#启动和停止svn)
- [在客户端中使用SVN](#在客户端中使用svn)
- [注](#注)
- [参考](#参考)

## 安装SVN

在Ubuntu中安装SVN，如：`sudo apt-get install subversion`。

## 建立SVN仓库

- 建立svn目录：`sudo mkdir /home/.svn`(使用隐藏目录)。
- `cd /home/.svn`。
- 创建文件夹：`mkdir lcfu1`。
- 创建仓库：`sudo svnadmin create /home/.svn/lcfu1`。

## 配置和管理SVN

- `cd lcfu1/conf`。

- `sudo vi svnserve.conf`，修改svnserve.conf配置文件，去掉下面4行前面的#符号。

  - anon-access = read
  - auth-access = write
  - password-db = passwd
  - authz-db = authz

  得到如下结果：

  ```
  ### This file controls the configuration of the svnserve daemon, if you
  ### use it to allow access to this repository.  (If you only allow
  ### access through http: and/or file: URLs, then this file is
  ### irrelevant.)
  
  ### Visit http://subversion.apache.org/ for more information.
  
  [general]
  ### The anon-access and auth-access options control access to the
  ### repository for unauthenticated (a.k.a. anonymous) users and
  ### authenticated users, respectively.
  ### Valid values are "write", "read", and "none".
  ### Setting the value to "none" prohibits both reading and writing;
  ### "read" allows read-only access, and "write" allows complete 
  ### read/write access to the repository.
  ### The sample settings below are the defaults and specify that anonymous
  ### users have read-only access to the repository, while authenticated
  ### users have read and write access to the repository.
  anon-access = read
  auth-access = write
  ### The password-db option controls the location of the password
  ### database file.  Unless you specify a path starting with a /,
  ### the file's location is relative to the directory containing
  ### this configuration file.
  ### If SASL is enabled (see below), this file will NOT be used.
  ### Uncomment the line below to use the default password file.
  password-db = passwd
  ### The authz-db option controls the location of the authorization
  ### rules for path-based access control.  Unless you specify a path
  ### starting with a /, the file's location is relative to the
  ### directory containing this file.  The specified path may be a
  ### repository relative URL (^/) or an absolute file:// URL to a text
  ### file in a Subversion repository.  If you don't specify an authz-db,
  ### no path-based access control is done.
  ### Uncomment the line below to use the default authorization file.
  authz-db = authz
  ### The groups-db option controls the location of the groups file.
  ### Unless you specify a path starting with a /, the file's location is
  ### relative to the directory containing this file.  The specified path
  ### may be a repository relative URL (^/) or an absolute file:// URL to a
  ### text file in a Subversion repository.
  #groups-db = groups
  ### This option specifies the authentication realm of the repository.
  ### If two repositories have the same authentication realm, they should
  ### have the same password database, and vice versa.  The default realm
  ### is repository's uuid.
  # realm = My First Repository
  ### The force-username-case option causes svnserve to case-normalize
  ### usernames before comparing them against the authorization rules in the
  ### authz-db file configured above.  Valid values are "upper" (to upper-
  ### case the usernames), "lower" (to lowercase the usernames), and
  ### "none" (to compare usernames as-is without case conversion, which
  ### is the default behavior).
  # force-username-case = none
  ### The hooks-env options specifies a path to the hook script environment 
  ### configuration file. This option overrides the per-repository default
  ### and can be used to configure the hook script environment for multiple 
  ### repositories in a single file, if an absolute path is specified.
  ### Unless you specify an absolute path, the file's location is relative
  ### to the directory containing this file.
  # hooks-env = hooks-env
  
  [sasl]
  ### This option specifies whether you want to use the Cyrus SASL
  ### library for authentication. Default is false.
  ### This section will be ignored if svnserve is not built with Cyrus
  ### SASL support; to check, run 'svnserve --version' and look for a line
  ### reading 'Cyrus SASL authentication is available.'
  # use-sasl = true
  ### These options specify the desired strength of the security layer
  ### that you want SASL to provide. 0 means no encryption, 1 means
  ### integrity-checking only, values larger than 1 are correlated
  ### to the effective key length for encryption (e.g. 128 means 128-bit
  ### encryption). The values below are the defaults.
  # min-encryption = 0
  # max-encryption = 256
  ```

- `sudo vi passwd`，修改passwd配置文件，这是每个用户的密码文件，就是“用户名=密码”，如下：lcfu1=12345678。

  ```
  ### This file is an example password file for svnserve.
  ### Its format is similar to that of svnserve.conf. As shown in the
  ### example below it contains one section labelled [users].
  ### The name and password for each user follow, one account per line.
  
  [users]
  # harry = harryssecret
  # sally = sallyssecret
  lcfu1=12345678
  ```

- `sudo vi authz`，修改authz配置文件，如下：

  - [groups]：为了便于管理,可以将一些用户放到一个组里边，如：lcfu1=lcfu1。
  - [/]：对一个目录的认证规则，比如对根目录的认证规则为[/]，设置单用户的认证规则时一个用户一行，如：lcfu1=rw。

  ```
  ### This file is an example authorization file for svnserve.
  ### Its format is identical to that of mod_authz_svn authorization
  ### files.
  ### As shown below each section defines authorizations for the path and
  ### (optional) repository specified by the section name.
  ### The authorizations follow. An authorization line can refer to:
  ###  - a single user,
  ###  - a group of users defined in a special [groups] section,
  ###  - an alias defined in a special [aliases] section,
  ###  - all authenticated users, using the '$authenticated' token,
  ###  - only anonymous users, using the '$anonymous' token,
  ###  - anyone, using the '*' wildcard.
  ###
  ### A match can be inverted by prefixing the rule with '~'. Rules can
  ### grant read ('r') access, read-write ('rw') access, or no access
  ### ('').
  
  [aliases]
  # joe = /C=XZ/ST=Dessert/L=Snake City/O=Snake Oil, Ltd./OU=Research Institute/CN=Joe Average
  
  [groups]
  # harry_and_sally = harry,sally
  # harry_sally_and_joe = harry,sally,&joe
  lcfu1=lcfu1
  
  # [/foo/bar]
  [/]
  lcfu1=rw
  # harry = rw
  # &joe = r
  # * =
  
  # [repository:/baz/fuz]
  # @harry_and_sally = rw
  # * = r
  ```

## 启动和停止SVN

- 从`.svn`目录启动：`svnserve -d -r /home/.svn`，就可以使用`svn://120.77.47.101/lcfu1`的方式来访问。
- 从lcfu1目录启动 ：`svnserve -d -r /home/.svn/lcfu1`，就可以使用`svn://120.77.47.101`的方式来访问。
- 检查svn服务器是否已经启动(svn默认使用3690端口)：`netstat -an | grep 3690`。
- 可使用`killall svnserve`停止svn服务。

## 在客户端中使用SVN

- 下载TortoiseSVN并安装：[https://tortoisesvn.net/downloads.html](https://tortoisesvn.net/downloads.html)。

- 在电脑中新建一个文件夹，点击鼠标右键，会看到如下：

  ![RightClick.PNG](https://raw.githubusercontent.com/lcfu1/Note/master/Aliyun/image/RightClick.PNG)

- 点击SVN Checkout，在URL of repository中填上我们自己的repository地址，如下：

  ![SVNCheckout.PNG](https://raw.githubusercontent.com/lcfu1/Note/master/Aliyun/image/SVNCheckout.PNG)

- 因为我们在passwd配置文件中配置用户名和密码，所以还要填写用户名和密码，如下：

  ![Authentication.PNG](https://raw.githubusercontent.com/lcfu1/Note/master/Aliyun/image/Authentication.PNG)

## 注

我是在阿里云的服务器上安装SVN的，按以上操作建立好SVN仓库，在客户端中checkout时还是会出现Authorization failed错误，右键TortoiseSVN->Settings->Saved Data->Authorization Data，Clear后还是不行。

解决方法：

要在阿里云控制台添加`安全组规则`，如下：

![SafetyGroupRules.PNG](https://raw.githubusercontent.com/lcfu1/Note/master/Aliyun/image/SafetyGroupRules.PNG)

## 参考

- [https://www.aliyun.com/jiaocheng/136686.html](https://www.aliyun.com/jiaocheng/136686.html)