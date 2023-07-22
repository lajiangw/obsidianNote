当遇到 RabbitMQ 服务名无效时，在管理员状态下进入 sbin 目录。在 cmd 窗口输入
第 1 步: 执行 rabbitmq-service. Bat remove
第 2 步: 执行 set RABBITMQ_BASE=D:\rabbitmq_server\data (路径自己定义)
第 3 步: 执行 rabbitmq-service. Bat install
第 4 步: 执行 rabbitmq-plugins enable rabbitmq_management


————————————————
版权声明：本文为 CSDN 博主「AshesInWord」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接： https://blog.csdn.net/AshesInWord/article/details/106107039
