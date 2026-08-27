校园生活服务平台项目介绍：
•面向高校师生的校园生活服务平台，涵盖跑腿代办、二手担保交易、宿舍报修、校园公告/活动、钱包与银行卡等核心模块；


所用技术： 
1. Spring Boot 3.4.1、MyBatis-Plus 3.5.11、Redis、MySQL、JWT、Spring AOP、Quartz、WebSocket、 
MapStruct、腾讯云 COS、Spring Security Crypto 的 BCrypt、Knife4j、Hutool、Lombok 
2.职责描述： 
3.业务模块开发：独立完成服务订单、二手担保交易、宿舍报修、校园公告/活动、用户钱包与银行卡等后端模块的接 
口设计、编码实现与接口文档输出。 
4.支 付 安 全 与 幂 等 ： 通 过 Spring AOP + Redis 实 现 支 付 接 口 幂 等 控 制 ， 以 订 单 号 为 Key 维 护 
PROCESSING/SUCCESS 状态，彻底杜绝重复扣款，保障资金一致性。 
5. 原子余额更新：将余额扣款改造为数据库原子 SQL：`UPDATE user SET balance = balance + #{amount} 
WHERE ... AND balance + #{amount} >= 0`，解决并发场景下余额扣成负数的问题。 
6. Redis 缓存防护：为公告/活动热点接口引入缓存穿透（空值占位）、缓存击穿（分布式锁单线程重建）、缓存雪崩 
（随机 TTL）三重防护，显著降低数据库峰值压力。 
7. 安全认证升级：基于 JWT 实现无状态登录认证，并引入 Token 黑名单机制；用户登出后将当前 Token 写入 
Redis 并跟随 Token 剩余有效期过期，实现 JWT 的主动失效。 
8. 实时订单推送：基于 Spring WebSocket 维护 userId -> WebSocketSession 映射，订单创建/支付/接单/ 
完成时通过 AOP 后置通知向下单人与接单人实时推送 JSON 状态消息。 
9. 持久化定时任务：引入 Quartz JDBC JobStore 替代 @Scheduled，每 5 分钟扫描并取消超时未支付/配送订 
单，任务状态持久化到数据库，支持集群部署与重启不丢失。 
10. 文件上传 OSS：将头像上传改造为对接腾讯云 COS，实现文件云端存储，降低本地服务器存储与带宽压力。 
11. 代码质量与规范：新增模块使用 MapStruct 完成 DTO 与实体高效转换，编译期生成代码避免反射损耗，使用 
BCrypt 替代 MD5 加密；新增接口全部通过 JSR303 校验，异常由 GlobalExceptionHandler 统一拦截返回。



项目目录说明：
——backend            #SpringBoot后端完整代码
——miniprogram        #微信小程序前端代码
——docs               #数据库SQL脚本、项目文档    
