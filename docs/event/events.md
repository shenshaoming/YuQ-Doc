# 内置事件列表
子事件会继承父事件属性，如无特殊情况，则不标注父事件含有的属性。

## 内置事件列表
事件名 | 父事件 | 可否取消 | 事件属性 | 描述
:---: | :---: | :---: | --- | ---
MessageEvent | Event | 是 | sender: 发送人 | 收到消息事件。
GroupMessageEvent | MessageEvent | 是 | sender: 发送消息的群成员, group: 消息来自群| 群消息事件。
PrivateMessageEvent | MessageEvent | 是 | - | 私聊消息事件。
- | - | - 
MessageRecallEvent | Event | 否 | sender: 发送者, operator: 撤回者, messageId: 消息Id| 消息撤回事件
PrivateRecallEvent | MessageRecallEvent | 否 | - | 私聊消息撤回事件
GroupRecallEvent | MessageRecallEvent | 否 | group: 撤回消息的群 | 私聊消息撤回事件
- | - | -
FriendListEvent | Event | 否 | - | 好友列表变动事件
FriendAddEvent | FriendListEvent | 否 | friend: 新增的好友 | 好友新增事件
FriendDeleteEvent | FriendListEvent | 否 | friend: 消失的好友 | 好友减少事件
- | - | -
GroupListEvent | Event | 否 | - | 群列表变动事件
BotJoinGroupEvent | GroupListEvent | 否 | group: 进入的群 | 机器人加入新群事件
BotLevelGroupEvent | GroupListEvent | 否 | group: 离开的群 | 机器人从离开某群事件
- | - | -
GroupMemberEvent | Event | 否 | group: 成员变动的群, member: 变动的群成员 | 群成员事件
GroupMemberJoinEvent | GroupMemberEvent | 否 | - | 新成员入群事件
GroupMemberInviteEvent | GroupMemberJoinEvent | 否 | inviter: 邀请者 | 新成员被邀请入群事件
GroupMemberLeaveEvent | GroupMemberEvent | 否 | - | 群成员退群事件
GroupMemberKickEvent | GroupMemberLeaveEvent | 否 | operator: 操作者 | 群成员被移除群事件
GroupBanMemberEvent | GroupMemberEvent | 否 | operator: 操作者, time: 禁言时长 | 群成员被禁言事件
GroupUnBanMemberEvent | GroupMemberEvent  | 否| operator: 操作者| 群成员被取消禁言事件
GroupBanBotEvent | GroupMemberEvent | 否 | operator: 操作者, time: 禁言时长 | 机器人在某群被禁言事件
GroupUnBanBotEvent | GroupMemberEvent | 否 | operator: 操作者| 机器人在某群被取消禁言事件
- | - | -
ContextSessionCreateEvent | Event | 否 | session: Session | ContextSession 创建事件。
- | - | -
ActionContextInvokeEvent | Event | 是 | context: 上下文 | Controller 处理链路事件
ActionContextInvokeEvent.Per | ActionContextInvokeEvent | 是 | - | Controller 处理之前事件
ActionContextInvokeEvent.Post | ActionContextInvokeEvent | 是 | - | Controller 处理之后事件

## 内置事件注意事项

事件名 | 注意事项
:--- | ---
MessageEvent | MessageEvent 及其子事件，取消事件会导致 Controller 部分一并取消响应。
GroupBanBotEvent | 与 GroupBanMemberEvent 事件相互独立，触发本事件不会触发 GroupBanMemberEvent 事件。
GroupUnBanBotEvent | 与 GroupUnBanMemberEvent 事件相互独立，触发本事件不会触发 GroupUnBanMemberEvent 事件。
ActionContextInvokeEvent.Per | 事件发生于路由寻路之前，也就是说，没有任何路由绑定依旧会触发此事件。取消会中断处理链路。
ActionContextInvokeEvent.Post | 事件发生于路由寻路之后，也就是说，没有任何路由绑定依旧会触发此事件。取消会中断可能存在的返回消息的发送。