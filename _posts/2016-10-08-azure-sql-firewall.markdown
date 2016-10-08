---
published: true
title: Azure SQL Server Firewall and Connection Problems
layout: post
comments: true
categories: azure
---
ถ้าเราใช้ SQL Server Database บน Microsoft Azure แล้วต้องการเข้าไปทำงานบน Database จากในเครื่องของเรา
บางครั้งอาจจะไม่สามารถ connect เข้าไปได้ ลองมาดูกันว่าสาเหตุมักจะมาจากอะไร แล้วจะแก้ไขได้อย่างไร

<!-- break -->

## ที่มา
ถ้าเรา connect ไปยัง Database Server ที่อยู่บน Microsoft Azure จากเครื่องของเราแล้วเจอ error แบบนี้

![Firewall is blocked][sql-firewall-blocked]

แสดงว่าบน Azure ไม่ได้เปิด Firewall ไว้สำหรับ IP ที่ระบุ
ซึ่งอาจจะพบปัญหาเดียวกันนี้เมื่อเราทดลองใช้งาน Application บนเครื่องของเราเองได้เหมือนกัน

แต่ถ้าเจอ error แบบอื่น ๆ เช่นภาพนี้ อาจจะไม่ได้หมายถึงเกิดจาก Firewall ซึ่งจะไม่ได้ลงรายละเอียดในบทความนี้ แต่สำหรับปัญหา Firewall เราจะอธิบายต่อไป

![Connection Error][sql-connection-error]

## Firewall?
ปกติ Server ที่ต้องการความปลอดภัยมาก (เช่น Database Server) มักจะใส่ Firewall เอาไว้ เพื่อไม่ให้ผู้ไม่ได้รับอนุญาตเช่น Hacker เข้ามาเจาะระบบเราได้ง่ายๆ
ดังนั้นการที่เกิดปัญหาไม่ได้เกิด Firewall แล้วไม่สามารถ connect เข้า SQL Server Database บน Azure ได้จึงเป็นเรื่องที่พบเห็นได้บ่อย ๆ สำหรับมือใหม่
แต่ถ้าเรามีความจำเป็นต้องเข้าไปใช้งานตัว Database จริง ๆ ก็มักจะทำกัน 2 วิธีคือ

1. สร้างเครื่องบน Azure ที่สามารถมองเห็นตัว Database ได้ แล้ว remote เข้าไปทำงานบนนั้น
2. เปิด Firewall เพื่อเข้าไปทำงาน เสร็จแล้วก็ปิดเมื่อเลิกใช้งาน

ซึ่งเราจะแนะนำวิธีที่ง่ายกว่าคือวิธีหลัง

## ขั้นตอน
วิธีการก็ง่ายมาก คือเข้าไปที่ Microsoft Azure แล้วไปยังตัว Database Server ของเราที่ต้องการทำงานด้วย เลือก Menu `Firewall` ระบุ Client IP ที่ต้องการจะเพิ่มเข้าไป
(ซึ่งหน้านี้จะบอกด้วยว่าเราใช้ IP Address อะไร แถมยังมีปุ่มให้เพิ่ม IP นี้เข้าไปแบบง่าย ๆ อีกด้วย) จากนั้นก็กด `Save` เป็นอันเรียบร้อย

![Set Firewall Rules][azure-sql-firewall-rules]

เมื่อใช้งานเสร็จแล้วก็กลับไปที่หน้าเดิมเพื่อกดปุ่ม `...` ที่ด้านหลังของเราเพื่อกดลบ แล้วก็กด `Save` อีกทีเป็นอันจบการทำงานอย่างสมบูรณ์

## ส่งท้าย
เราได้แนะนำวิธีง่าย ๆ ที่จะเข้าไปทำงานกับ Database ที่ปกติจะปิด Firewall เอาไว้ไม่ให้คนทั่วไปเข้าถึงได้ด้วยการเปิด Firewall
จะเห็นว่าวิธีการง่ายมาก แต่เมื่อใช้เสร็จแล้ว***อย่าลืมลบตัว Firewall ที่เราเปิดไว้ทิ้งด้วย***เพื่อความปลอดภัย

[sql-firewall-blocked]: /imgs/azure-sql-firewall-blocked-dialog.png "Dialogbox Alert about Firewall Rules"
[sql-connection-error]: /imgs/azure-sql-connection-error.png "Other Connection Error"
[azure-sql-firewall-rules]: /imgs/azure-sql-firewall-rules.png "Set Firewall Rules"