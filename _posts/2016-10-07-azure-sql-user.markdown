---
published: true
title: Database Logins
layout: post
comments: true
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

[db-blade]: /imgs/azure-sql-db-blade.png "From Azure Management Portal"
[db-server-name]: /imgs/azure-sql-db-server-name.png "From Azure Management Portal"
[mgr-studio-login]: /imgs/sql-mgr-studio-login.png "SQL Server Management Studio login screen"
[sql-firewall]: /imgs/azure-sql-firewall-blocked.png "Firewall rule is not allowed to access the DB Server"

