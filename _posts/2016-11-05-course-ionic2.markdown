---
published: true
title: (Course) Ionic2 Bootcamp
layout: post
comments: true
categories: course
---

# A Complementary Course Material
ใช้ประกอบการอบรมและทำ workshop สำหรับ Ionic2 Bootcamp

<!-- break -->

Workshop material's available at: [https://tlaothong.gitbooks.io/ionic2-bootcamp/content/](https://tlaothong.gitbooks.io/ionic2-bootcamp/content/)

# Setup Links
* Download [Node.js](https://nodejs.org/)
* Install [Ionic2](http://ionicframework.com/docs/v2/getting-started/installation/)
* [Android Platform Guide](https://cordova.apache.org/docs/en/latest/guide/platforms/android/)
* [iOS Platform Guide](https://cordova.apache.org/docs/en/latest/guide/platforms/ios/)

# Resources
[Ionic2 Document](http://ionicframework.com/docs/v2/)
[Binding Syntax (ฉบับย่อ)](http://learnangular2.com/templates/)
[Angular2 Template Syntax (Binding ฉบับเต็ม)](https://angular.io/docs/ts/latest/guide/template-syntax.html)

# Assignment #1

## การเตรียมตัว

1. เข้าไปที่ [https://github.com/tlaothong/ionic2-bootcamp](https://github.com/tlaothong/ionic2-bootcamp) ที่มีทั้ง demo และ source ที่ใช้สำหรับการบ้าน
* ตัว demo อยู่ใน folder demo / firstdem ซึ่งถ้าเข้าไปที่ folder demo จะบอกวิธีการเตรียม source code เพื่อให้ run firstdem (หรือตัวอย่างอื่น ๆ ได้)

## รายละเอียด

1. สร้าง ionic 2 application โดยใช้ template `tabs` ตั้งชื่อ project ตามใจ สำหรับคำสั่งตัวอย่างตั้งชื่อว่า assignment1

    ionic start assignment1 tabs --v2

* ใน folder `src/pages/home` ให้เปิดไฟล์ `home.ts` ประกาศตัวแปรชื่อ `actor` มากำหนดค่าที่ได้มาจาก source code ในไฟล์ที่ download มาชื่อ `actor.json` ด้านล่างคือตัวอย่างที่ตัดมาบางตอน

    public actor = [
        {
            "id": 1,
            ...
        },
        ...
    ];

* ใน folder เดียวกัน (`src/pages/home`) ให้เปิดไฟล์ `home.html` แล้วดำเนินการต่อเพื่อให้ได้ผลลัพธ์ต่อไปนี้
* สร้าง list เพื่อนำข้อมูลในตัวแปร `actor` มาแสดงในหน้า home ให้มีความสวยงามตามสมควร
* ข้อมูล `mentalScore` ของแต่ละ `actor` ให้แสดงโดยใช้ `badge` และแสดงสีตามค่าตัวเลขโดยถ้าตัวเลขต่ำกว่า 90 ให้มีสีแบบ `danger` แต่ถ้า 90 ขึ้นไปให้ใช้สีแบบ `secondary`

# Preparation for the next week
1. Setup Visual Studio
    * มีคำแนะนำเพิ่มเติมที่ดูได้จาก [Visual Studio Tools](http://blog.ionic.io/visual-studio-tools-for-apache-cordova/?_ga=1.95307038.38020642.1478473497)
* Setup Android SDK or iOS SDK (might need Mac)
    * ภาพรวมการ setup บนเครื่อง Windows ดูได้จาก [Windows Setup](http://ionicframework.com/docs/v2/resources/platform-setup/windows-setup.html)
    * รายละเอียดของการติดตั้ง Android SDK [Android Platform Guide](https://cordova.apache.org/docs/en/latest/guide/platforms/android/)
    * สำหรับเครื่อง Mac อาจติดตั้ง [iOS Platform Guide](https://cordova.apache.org/docs/en/latest/guide/platforms/ios/)
