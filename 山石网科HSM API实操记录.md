## 1.通过“配置下发工单”下发策略
待添加策略：

  rule id 2
    action permit
    src-zone "trust"
    dst-zone "trust"
    src-ip 1.1.1.2/32
    dst-ip 1.1.1.2/32
    service "HTTP"
  exit
### 1.1 创建工单
请求方法:POST
请求路径：http://[ip]/api/ticket/cli-ticket
请求示例：
{
    "operateType": 1,
    "ticket": {
        "ticketName": "ticket_1",
        "jobType": "task",
        "configList": [
            {
                "ip": [
                    "192.168.72.225"
                ],
                "command": ["rule id 2","action permit","src-zone trust","dst-zone trust","src-ip 1.1.1.2/32","dst-ip 1.1.1.2/32","service HTTP","exit"
                ]
            }
        ]
    }
}

响应示例：
{
    "data": {
        "ticketId": 17,   //注意生成的ID号，由于删除了之前创建的工单，所以ID号在一直增加
        "ticketName": "ticket_1"
    },
    "errorCode": null,
    "errorInfo": null,
    "status": true
}

### 1.2 批量审核工单
请求方法:POST
请求路径：http://[ip]/api/ticket/cli-ticket/review
请求示例：
{
    "operateType": 1,
    "ticketIdList": [ 17 ],   //该ID号为上次响应生成的ID
    "reviewMsg": "Pass",
    "reviewStatus": 0
}

### 1.3 批量下发工单
请求方法:POST
请求路径：http://[ip]/api/ticket/cli-ticket/issued
请求示例：
{
    "operateType": 1,
    "ticketIdList": [ 17 ],  //该ID号为上次响应生成的ID
    "isForced": true
}

### 1.4 查询下发工单历史
请求方法:GET
请求路径：http://[ip]/api/ticket/cli-ticket/issued/history
请求示例：http://192.168.72.224/api/ticket/cli-ticket/issued/history?ticketId=17  //请求方法为添加URL字段tichetId
