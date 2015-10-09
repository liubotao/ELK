## Logstash


### 起源

日志管理经常被认为是一个痛苦记录和黑科技.事实上，理解好的日志管理方法是一个漫长的过程.为了解决系统故障和问题,管理员总会告诉我们去看日志。所以cat,tail和grep组合命令操作日志成为诊断定位问题的法宝。他们迅速称掌握正则和命令行功夫:从大量的日志文件中搜索,分析,清理，提炼数据,对所有系统管理员来讲它是非常的实用和强大       

但是可惜的是,这种解决方案并不是十分高级,在大多数情况下,你有不止一个的主机和多种类型的日志,你可能拥有成千上万台主机,很多内部应用和服务都是跨机房调用。在这个情况下通常我们分析单台的日志不能帮我们解决这种复杂环境的问题      

为了解决这个问题,你必须将你的日志管理变成集中化管理,你可以通过使用rsyslog或者syslog-ng工具传输syslog日志,所有时间开始接受,大量的数据被建立,消耗大量的存储空间    

但是这个还远远不够.由于大量日志产生于不同的事情，不同的格式,或者不同语言.在快速增长的日志文件中你很难找到你需要的数据，快速增长采集的日志变成了负担

为了解决这个问题,你必须拓展日志管理方法以便更加方便的解析这些日志,更加优雅的日志存储方便索引和检索.我们把一个简单的grep操作日志变成了我们的一个主要项目.这个项目融合了不同的工具来产生新的解决方案

### 简介

Logstash为日志采集，中性化，解析，存储和搜索提供了一体化框架,Logstash是免费和开源的,其作者是一个美国人.它部署起来非常简单,方便和高效,其外它也是非常容易拓展的  

LogStash提供多种输入机制:它能接受TCP/UDP, 文件, Syslog, Microsoft Windows EventLogs, STDIN和其他的源输入,所以基本上你的系统都能够很好的接入它   

当大量的日志到达LogStash服务,你可以选择不通的过滤器,这些过滤器可以改变,操作和传输这些数据,你能从这些数据事件中来抽出你想要的信息,LogStash处理这些时间是非常简单的,它能简单的描述的结果让你对这些数据做出好的处理选择

最后,当数据输出的时候,LogStash同样支持不同格式的输出,包括TCP/UDP,email,文件,HTTP,Nagios和其他多种网络在线服务,你能使用LogStash将提醒服务,报备等服务整合成一体化   

###  设计原理和风格

LogStash是使用JRuby编写,运行于Java虚拟机的应用,它的设计是基于消息的,相比其他需要另外依赖的代理和服务的应用，LogStash是非常简单的，LogStash也只有一个单一的代理,你能通过配置不同的方法来组合不通的开源组件  

LogStash的四个组件构成了它的生态环境  

*  Shipper: 发送事件流,远程的代理只需要运行此组件 
*  BrokerAndIndexer:  接受信息流并且进行索引
*  SearchAndStorage: 允许你进行存储和搜索信息流
*  WebInterface： 一个Web界面来显示信息流程 

LogStash的服务器可以独立的运行一个或者多个组件,它允许我们将不同的组件进行分离来提高其效率    

通常情况下.我们对LogStash的规划如下:

*  运行LogStash代理扮演Shipper角色,它的的主要任务是应用,服务和主机消息事件发送到中心的LogStash服务器,他们只需要运行LogStash代理
*  中心的LogStash服务运行代理,索引，搜索和存储和web界面服务，他们用来处理,接受和存储日志

### 安装使用


安装java环境

Red Hat系统运行   

    $ sudo yum install java-1.7.0-openjdk
  
Debian&Ubuntu

     $sudo apt-get -y install openjdk-7-jdk
     
下载,安装LogStash

    $wget https://download.elastic.co/logstash/logstash/logstash-1.5.4.zip
    $unzip logstash-1.5.4.zip
    $sudo mkdir /data/logstash
    $sudo cp -rf logstash-1.5.4/* logstash/
    $sudo mkdir /etc/logstash/logstash.conf
    
使用LogStash    
一旦我们有了jar file文件我们能其他jar包文件配置一个简单的配置文件  
首先我们创建一个鸡蛋的配置文件,我们命名为sample.conf,配置文件内容如下:

	input { stdin { } } output { stdout {} }
  
  我们的sample.conf文件包含2个重要的配置快:一个叫输入input 一个叫输出,他们是3个LogStash服务器组件中的2个,另外一个filter组件给我们会在后面章节再介绍.每个组件的配置和含义如下: 
  
  *  input -  信息流怎么进入LogStash  
  *  filters - 我们怎么操作和处理LogStash的信息流
  *  outputs - 我们将处理之后的信息流发往输出
  
在LogStash的世界里,信息流通过inputs组件输入,通过filters组件操作和转变数据流,最后通过outputs组件完成这次的处理 
  
在LogStash内部每个组件都可以单独配置,比如，input模块,我们可以定义标准的stdin插件,LogStash就会接受标准STDIN输入,在output模块种我们可以配置相反的,stdout组件,它能输出事件到标准输出,在我们刚才配置种我们加上一个选项:debug 当前每个输出都可以看成一个json hash
  
运行LogStash代理   
 
现在我们获取一个配置文件然后运行LogStash    
 
     $/opt/logstash/bin/logstash agent -v -f /etc/logstash/logstash.conf 
          
 Note: 每次修改LogStash配置都需要重启LogStash才能读取新的配置   
 
 我们使用java -jar命令来运行下载的包,我们还使用到一个agent标识来告诉LogStash作为最基础的代理运行, -v表示打开详细的日志信息,-f选项来指定LogStash配置文件地址  
 
  我们可以使用-vv 参数看到更加详细的输出信息
  
  LogStash会马上运行并且产生一条启动信息,他会告诉我们我们开启了插件，其输出如下:    
  
    Pipeline started {:level=>:info}
    Logstash startup completed
    hello world
    2015-10-09T03:39:56.192Z MacBook-Pro.local hello world

  他表示LogStash开始准备接受处理日志
  
  LogStash在接受输入之前需要一段时间,如果他非常慢的话我们可以调整java的最新和最大heap大小来获取更多的内存来处理这个程序,我们可以使用-Xms和-Xmx参数,-Xms参数告诉我们初始的分配内存大小,-Xmx参数定义系统进程能够使用最大内存.我们可以定义如下:
  
     $ java -Xms384m -Xmx384m -jar logstash.jar agent -v -f sample.conf
     
这个设置表示最大和最小的内存分配都是384M，分配更多的内存给LogStash他会提高其处理速度,但是由于java gc的原因使用有不可预测的情况发生,实际过程中,Java和JVM的调整学习过程会有一个曲折,保证代理和其他组件分离是一个提高N高可用的方法    

测试LogStash代理  

现在Logstash是运行的,记的我们开启了stdin组件？LogStash现在正在等待一些STDIN输入,所以我们通过输入"testing"看看有什么发生

我们能看到我们的输入已经进入LogStash并且被输出了:一条从LogStash来的info等级的信息和一条Json格式信息(我们打开了debug选项为标准输出插件)，下面让我们解释更多细节    

我们能看到我们的时间信息流有一个timestamp时间戳,当前主机产品了一条xxx.explage.com的信息,在我们测试过程中你会发现所有的组件输出都i在#data哈希中    

我们能看到我们的信息流流当成一个hash，实际上他是在LogStash是当成一个JSON hash

如果我们在stdout插件中也打开debug选项,我们会看到如下输出  

LogStash称这些格式为编码(codecs),LogStash本身支持很多不一样的编码,我们继续会看到原生的和json编码之后的    

*  plain - 信息流被看成一个原始的文件,任何解析器都可以使用filter插件来解析它
*  json - 信息流被认定为json数据,LogStash尝试去解析这些信息流内容

我们会继续关注到json这种格式,因为他是LogStash能够处理最简单的格式,一个基本的信息流包括下面2个元素:

*   @timstamp: ISO08601 timstamp
*   message 时间消息,这个我们测试testing我们输入到STDIN
*   @version 格式化信息流的版本,当前版本是1

Tip: 当我们直接运行Logstash的时候 我们可以使用Ctril+c来停止LogStash服务
 
### 配置格式


当你想添加事件消息处理管道时,Logstash的配置文件每一个组件有一个独立的分开的段,比如:

    input {
	...
	}
	
	filter {
	...
	}
	
	output {
	...
	}

 每一个块包含配置一个或者多个插件,如果你需要多个过滤插件,他们必须按照顺序配置在文件中.
 
 每一个插件块配置命名之后是插件定义的,比如,输入片段配置2个文件输入:
 
    input {
	   file {
	       path => '/var/log/messages'
		   type => 'syslog'
	   }
	   
	   file {
	       path => '/var/log/apache/access.log'
		   type => 'apache'
	   }
	}
 在这个demo种,2个设置选项在每个文件输入中:path和type    
 
 插件的配置允许一个类型,比如boolean和hash,如下的值类型也是支持的   
 
 Array 
 
 数组是可以是一个单个值也可以是多个值,如果你设置相同的选项多次，他也会被添加到数组中,
 
     path => ["/var/log/messages", "/var/log/*.log"]
     path => "/data/mysql/mysql.log"
     
 Boolean    
 Boolean类型必须是true或者fals,特别要注意的时候关键字true或者false不能够用双引号进行引用.

    ssl_enable = true
    
 Bytes   
 字节字段必须是一个合法的字节计数单元,使用方法如下:

    my_bytes = "1113"  # 1113bytes
    my_bytes = "10MiB" # 10485760 bytes
    my_bytes = "100kib" # 102400bytes
    my_bytes = "180 mb" # 1800000000bytes
  
 Codec
     
     codec => "json"
     
 Hash
 
     math => {
      "field1" => "value1"
      "filed2" => "value2"        
      }
 数字
 
     port => 33
     
 密码
 
     my_passport => "passowrd"
     
 路径 
 
     my_path => "/tmp/logstash"
     
 字符串
 
     name => "Hello world"
     
 注释
 
     #this is a comment 
     
     input { $comment can appear at the end of a line
        #...
     }
  
### 设置为启动服务

    #! /bin/sh
	
	# From The Logstash Book
	# The original of this file can be found at: http://logstashbook.com/code/index.html
	#
	
	### BEGIN INIT INFO
	# Provides:          logstash
	# Required-Start:    $remote_fs $syslog
	# Required-Stop:     $remote_fs $syslog
	# Default-Start:     2 3 4 5
	# Default-Stop:      0 1 6
	# Short-Description: Start daemon at boot time
	# Description:       Enable service provided by daemon.
	### END INIT INFO
	
	. /lib/lsb/init-functions
	
	name="logstash-central"
	logstash_bin="/opt/logstash/bin/logstash"
	logstash_conf="/etc/logstash/central.conf"
	logstash_log="/var/log/logstash/central.log"
	pid_file="/var/run/$name.pid"
	cwd=`pwd`
	
	start () {
			command="${logstash_bin} agent --verbose -f $logstash_conf --log $logstash_log"
	
			log_daemon_msg "Starting $name"
			if start-stop-daemon --start --quiet --oknodo -d /opt/logstash/ --pidfile "$pid_file" -b -m -N 19 --exec $command; then
					log_end_msg 0
			else
					log_end_msg 1
			fi
	}
	
	stop () {
			log_daemon_msg "Stopping $name"
			start-stop-daemon --stop --quiet --oknodo --pidfile "$pid_file"
	}
	
	status () {
			status_of_proc -p $pid_file "" "$name"
	}
	
	case $1 in
			start)
					if status; then exit 0; fi
					start
					;;
			stop)
					stop
					;;
			reload)
					stop
					sleep 2
					start
					;;
			restart)
					stop
					sleep 2
					start
					;;
			status)
					status && exit 0 || exit $?
					;;
			*)
					echo "Usage: $0 {start|stop|restart|reload|status}"
					exit 1
					;;
	esac
	
	exit 0
	
设置过程如下:

    $ sudo cp logstash /etc/init.d/logstash
    $ sudo chmod 755 /etc/init.d/logstash
    $ sudo chown root:root /etc/init.d/logstash
    
接下来 我们可以初始化脚本让服务能够使用它

    $ sudo update-rc.d logstash enable
    $ sudo /etc/init.d/logstash start
    * logstash is not running
    * Staring logstash
 
检查logstash服务是否开启

    $ /etc/init.d/logstash status
    * logstatsh is running
    
    
### 使用Syslog


### 高可用高性能


*  Kafka - 作为接受数据的代理服务
*  ElasticSearch - 搜索或者存储数据
*  LogStash - 消息和索引信息

下面是介绍的几点来提高这些组件高可用

*   提供多个LogStash避免单点故障 
*   在input和output处理过程中,尽量避免丢失数据 
*   提供LogStash的性能

Warnning:不是所有的性能参数调整方法都是适合你的系统,我们能提供基本的设置来提高Logstash的处理能来,但是在你实际项目里面你必须调整参数并且反复观察信息来确认这些参数调整是符合你需要的

 
