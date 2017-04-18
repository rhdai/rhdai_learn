## 一、背景
该项目主要业务都是按照流程定义运作的，出版各种图书都是特定的流程，整个出版过程分解若干小过程后，也有很多共同的！因此项目把各种出版的流程分解成一步一步小流程，小流程中包含具体的操作。
## 二、设计
 - 目的：为了尽量精简和重用，避免每个流程都去写逻辑，我们抽取出流程的主线，基本所有流程的开始，完成，结束都设计成通用。
 - 为此主要涉及4张表：
1. 流程表(SP_PROCESS)
    ```
    CREATE TABLE "DB_SP"."SP_PROCESS" 
       (	"PROCESS_ID" NUMBER NOT NULL ENABLE, 
    	"PROCESS_NAME" VARCHAR2(300), 
    	"PRESS_ID" NUMBER, 
    	"KEY" VARCHAR2(500), 
    	"STATUS" NUMBER(1,0), 
    	"BPMN_PATH" VARCHAR2(1000), 
    	"BPMN_IMG" VARCHAR2(1000), 
    	"CREATOR" NUMBER, 
    	"CREATE_DATE" TIMESTAMP (7), 
    	"TYPE" NUMBER(4,0), 
    	"DEPLOY_ID" VARCHAR2(200), 
    	"TASK_JSON" VARCHAR2(3000), 
    	 CONSTRAINT "PK_SP_PROCESS" PRIMARY KEY ("PROCESS_ID")
    ```
    i.该表主要是基本小流程表,主要记录流程的信息啊
    
2. 流程定义表(SP_PROCESS_DEFINE)
    ```
    CREATE TABLE "DB_SP"."SP_PROCESS_DEFINE" 
        (	"TASK_ID" VARCHAR2(100) NOT NULL ENABLE, --activiti节点的key
    	"ROLE_ID" NUMBER NOT NULL ENABLE, 
    	"PROCESS_ID" NUMBER NOT NULL ENABLE, 
    	 CONSTRAINT "PK_SP_PROCESS_DEFINE" PRIMARY KEY ("TASK_ID", "ROLE_ID", "PROCESS_ID")
    ```
    i.定义流程表中的task_json（就是activiti中的任务节点）的权限，通俗点就是定义这步操作谁来执行完成
    
3. 流程规则
    ```
    CREATE TABLE "DB_SP"."SP_PROCESS_RULE" 
   (	"RULE_ID" NUMBER NOT NULL ENABLE, 
	"TYPE" NUMBER(1,0), 
	"BUSI_TYPE" NUMBER(1,0), 
	"KEY" VARCHAR2(255), 
	"BPMN_PATH" VARCHAR2(1000), 
	"BPMN_IMG" VARCHAR2(1000), 
	"STATUS" NUMBER(1,0), 
	"PRESS_ID" NUMBER, 
	"DEPLOY_ID" VARCHAR2(300), 
	"TASK_JSON" VARCHAR2(4000), 
	"CREATE_DATE" TIMESTAMP (7), 
	"CREATOR" NUMBER, 
	"RULE_NAME" VARCHAR2(255), 
	 PRIMARY KEY ("RULE_ID")
    ```
    i.其实本质和流程表是一样的，区别在于流程规则表的task节点key就是流程表中流程的key
    


