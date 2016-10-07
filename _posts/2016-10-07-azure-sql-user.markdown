---
published: true
title: Database Logins
layout: post
comments: true
tags: sql, azure, login, security
---
ในการพัฒนา Application มีหลายครั้งที่เราสร้าง Database มาใช้ร่วมกัน จะได้ไม่ต้องวุ่นวายกับ Schema ที่เปลี่ยนไป
แต่การใส่ Connection String ที่มี Username กับ Password ไว้ใน Source Code Repository อาจจะดูไม่ปลอดภัยเท่าใดนัก
โดยเฉพาะหากเราใส่ Master User เช่น `sa` ที่เข้าถึงข้อมูลได้ทุกอย่าง เท่ากับเปิดประตูต้อนรับผู้ไม่หวังดี
หัวข้อนี้เราจะแนะนำวิธีที่จะหลีกเลี่ยงการใส่ `sa` ไว้ใน Connection String

<!-- break -->

## แนวทางที่เสนอ
วิธีหลีกเลี่ยงอาจมีหลายแนวทาง ในหัวข้อนี้จะแนะนำการสร้าง User เฉพาะสำหรับ Application นั้น ๆ ขึ้นมา แล้วใช้ User นี้แทน `sa` ถ้ามีผู้ไม่หวังดีได้ User นี้ไป
ก็จะจัดการอะไรต่าง ๆ ได้แค่ Database ตัวนั้น ๆ เท่านั้น ไม่ใช่กับ Server ทั้งเครื่อง และเราสามารถทิ้ง User ใน Database นั้นไปให้หมด แล้วสร้างขึ้นมาใหม่ได้โดยไม่กระทบกับ Application อื่น ๆ

ดังนั้นเราจะมาดูวิธีการต่อไปนี้

1. เตรียมตัว
2. สร้าง User ที่มีสิทธิ์เฉพาะ Database ที่เราต้องการทำงานด้วย
3. ลอง login เพื่อยืนยันว่า User ดังกล่าวใช้งาน Database นั้นได้

## เตรียมตัว
ในหัวข้อนี้เราจะใช้ `SQL Server` บน `Microsoft Azure` เป็นตัวอย่างในการทำขั้นตอนที่กล่าวมา ดังนั้นเราต้องเตรียมข้อมูลให้พร้อมว่า login ของ Database
มี Username, Password ว่าอะไร แล้วจึงเรียกโปรแกรมที่ใช้จัดการ Database อย่าง `SQL Server Management Studio` ขึ้นมาใช้งานตามขั้นตอนด้านล่าง

1. เตรียม Username และ Password ที่ใช้ login เข้า Database ให้เรียบร้อย
2. ดูว่า Database ของเรามีชื่อเครื่องเรียกว่าอะไร โดยใน `Azure` เข้าไปดูได้ในหน้า Portal ตามขั้นตอนดังนี้
    * เข้าไปดู blade ของ `SQL Database` แล้วเลือกตัว Database ที่เราต้องการ จะปรากฏ blade ย่อยดังภาพ
        
        ![SQL Database Blade][db-blade]

    * เลือก Properties เพื่อดู Server Name แล้ว copy เอาไว้ไปใช้ login

        ![SQL Database Server Name][db-server-name]

3. เปิดโปรแกรมที่ใช้จัดการ Database อย่าง `SQL Server Management Studio` ขึ้นมา แล้วใส่ข้อมูลที่เตรียมไว้เช่น Server Name, Username, Password เพื่อ login

    ![Login to Management Studio][mgr-studio-login]

4. หากมีปัญหาในการ login เช่น มีข้อความแปลก ๆ แจ้งขึ้นมาดังภาพด้านล่าง หมายความว่า Database บน `Azure` ไม่ได้อนุญาตให้เราเข้าถึงได้ ซึ่งแก้ได้ด้วยการเปิด firewall ของ Database ซึ่งวิธีการดูได้จาก บทความ

    ![Firewall is blocking][sql-firewall]

5. เมื่อเข้าใช้งาน `SQL Server Management Studio` ได้เรียบร้อยแล้ว ก็สามารถสร้าง User ต่อได้เลย

## สร้าง Login เข้า Database Server 
ต่อไปเราจะสร้าง User ที่มีสิทธิ์เฉพาะ Database ที่เราต้องการทำงานด้วย จะเริ่มจากสร้าง login เข้า Server ก่อนซึ่งจะกล่าวถึงในหัวข้อนี้ และหัวข้อถัดไปจะกล่าวถึงการนำ login ดังกล่าวไปกำหนดให้ login เข้า Database ต่อไป

1. ให้เลือกสร้าง `New Login...` จาก `Security` -> `Logins` ให้สังเกตว่า `Security` เป็น node ลูกที่ติดกับตัว Server เลยนะครับ ***ไม่ใช่* Security ที่อยู่ภายใต้ `Databases`** นะครับ

    ![New Login][server-new-login]

2. จะปรากฏ `SQL Statements` แบบนี้ ขึ้นมาแสดงบน Workspace

        -- ===============================================
        -- Create SQL Login template for Windows Azure SQL Database
        -- ===============================================

        CREATE LOGIN <SQL_login_name, sysname, login_name> 
            WITH PASSWORD = '<password, sysname, Change_Password>' 
        GO

3. ให้เราแก้ไขดังนี้

    | Statement | อธิบาย | ตัวอย่าง  |
    |---------------------------------------|---------------------------------------|--------------|
    | <SQL_login_name, sysname, login_name> | ให้เลือกใส่ username สำหรับ login ที่ต้องการ       | catuser |
    | <password, sysname, Change_Password> | ใส่ password สำหรับใช้ login   | C@t4P@ssw0rd |

4. `SQL Statements` ที่แก้ไขแล้วจะเป็นประมาณนี้ 

        -- ===============================================
        -- Create SQL Login template for Windows Azure SQL Database
        -- ===============================================

        CREATE LOGIN catuser 
            WITH PASSWORD = 'C@t4P@ssw0rd' 
        GO

5. แล้วสั่ง `Execute`

    ![Execute New Login][mgr-studio-execute-new-login]

6. จะมี Message ที่หน้าต่าง `Messages` ด้านล่างลงมา บอกว่า `Command(s) completed successfully.` ดังภาพ จบขั้นตอนนี้

    ![New Login Completed][mgr-studio-execute-new-login-completed]

## สร้าง User ที่มีสิทธิ์เฉพาะ Database ที่ต้องการ
หัวข้อนี้จะกล่าวถึงการนำ login เข้า Server ที่สร้างจากขั้นตอนที่แล้ว ไปสร้าง User ให้ใช้งาน Database ที่ต้องการได้

1. ให้เลือกสร้าง `New User...` จาก `Databases / ชื่อ Database` -> `Security` -> `Users` ให้สังเกตว่า `Security` เป็น node ลูกของตัว `Database` ที่เราต้องการนะครับ

    ![New User][server-new-user]

2. จะปรากฏ `SQL Statements` แบบนี้ ขึ้นมาแสดงบน Workspace

        -- =================================================
        -- Create User as DBO template for Windows Azure SQL Database
        -- =================================================
        -- For login <login_name, sysname, login_name>, create a user in the database
        CREATE USER <user_name, sysname, user_name>
            FOR LOGIN <login_name, sysname, login_name>
            WITH DEFAULT_SCHEMA = <default_schema, sysname, dbo>
        GO

        -- Add user to the database owner role
        EXEC sp_addrolemember N'db_owner', N'<user_name, sysname, user_name>'
        GO

3. ให้เราแก้ไขดังนี้

    | Statement | อธิบาย | ตัวอย่าง  |
    |---------------------------------------|---------------------------------------|--------------|
    | <user_name, sysname, user_name> | ตั้ง username ที่ต้องการเรียกใน database แนะนำให้ใช้ชื่อเดียวกับ login ในขั้นตอนที่แล้ว | catuser |
    | <login_name, sysname, login_name> | ชื่อ login ที่ตั้งจากขั้นตอนที่แล้ว | catuser |
    | <default_schema, sysname, dbo> | Schema ใน database ที่ต้องการให้เข้าถึงได้ หากต้องการให้ใช้ได้ทั้งฐานข้อมูลให้ใส่เป็น dbo | dbo |

4. `SQL Statements` ที่แก้ไขแล้วจะเป็นประมาณนี้ 

        -- =================================================
        -- Create User as DBO template for Windows Azure SQL Database
        -- =================================================
        -- For login <login_name, sysname, login_name>, create a user in the database
        CREATE USER catuser
            FOR LOGIN catuser
            WITH DEFAULT_SCHEMA = dbo
        GO

        -- Add user to the database owner role
        EXEC sp_addrolemember N'db_owner', N'catuser'
        GO

5. แล้วสั่ง `Execute`

    ![Execute New User][mgr-studio-execute-new-user]

6. จะมี Message ที่หน้าต่าง `Messages` ด้านล่างลงมา บอกว่า `Command(s) completed successfully.` ดังภาพ จบขั้นตอนนี้

    ![New User Completed][mgr-studio-execute-new-user-completed]


[db-blade]: /imgs/azure-sql-db-blade.png "From Azure Management Portal"
[db-server-name]: /imgs/azure-sql-db-server-name.png "From Azure Management Portal"
[mgr-studio-login]: /imgs/sql-mgr-studio-login.png "SQL Server Management Studio login screen"
[sql-firewall]: /imgs/azure-sql-firewall-blocked.png "Firewall rule is not allowed to access the DB Server"
[server-new-login]: /imgs/sql-mgr-studio-new-login.png "New Login to a DB Server"
[mgr-studio-execute-new-login]: /imgs/sql-mgr-studio-execute-new-login.png "Execute New DB Server Login"
[mgr-studio-execute-new-login-completed]: imgs/sql-mgr-studio-completed-execute-new-login.png "Execute New DB Server Login Completed"
[server-new-user]: /imgs/sql-mgr-studio-new-user.png "New User to the Database"
[mgr-studio-execute-new-user]: /imgs/sql-mgr-studio-execute-new-user.png "Execute New User in the Database"
[mgr-studio-execute-new-user-completed]: imgs/sql-mgr-studio-completed-execute-new-user.png "Execute New User into the Database Completed"