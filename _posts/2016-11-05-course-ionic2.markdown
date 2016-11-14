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
* [Ionic2 Document](http://ionicframework.com/docs/v2/)
* [Binding Syntax (ฉบับย่อ)](http://learnangular2.com/templates/)
* [Angular2 Template Syntax (Binding ฉบับเต็ม)](https://angular.io/docs/ts/latest/guide/template-syntax.html)

# Hands-Out
* [Mobile Apps Landscape](https://dl.dropboxusercontent.com/u/19631829/ionic2/m01.pdf)
* [TypeScript & Angular2](https://dl.dropboxusercontent.com/u/19631829/ionic2/m02.pdf)
* [Using Ionic](https://dl.dropboxusercontent.com/u/19631829/ionic2/m03.pdf)

# Workshop #2
* Execute the following command to create a tutorial project:

        ionic start workshop2 tutorial --v2

* Run and explore the source code

# Workshop #3
* [Ionic 2 Conference App](https://github.com/driftyco/ionic-conference-app)

# Assignment #1

## การเตรียมตัว

1. เข้าไปที่ [https://github.com/tlaothong/ionic2-bootcamp](https://github.com/tlaothong/ionic2-bootcamp) ที่มีทั้ง demo และ source ที่ใช้สำหรับการบ้าน
2. ตัว demo อยู่ใน folder demo / firstdem ซึ่งถ้าเข้าไปที่ folder demo จะบอกวิธีการเตรียม source code เพื่อให้ run firstdem (หรือตัวอย่างอื่น ๆ ได้)

## รายละเอียด

1. สร้าง ionic 2 application โดยใช้ template `tabs` ตั้งชื่อ project ตามใจ สำหรับคำสั่งตัวอย่างตั้งชื่อว่า assignment1

        ionic start assignment1 tabs --v2

2. ใน folder `src/pages/home` ให้เปิดไฟล์ `home.ts` ประกาศตัวแปรชื่อ `actors` มากำหนดค่าที่ได้มาจาก source code ในไฟล์ที่ download มาชื่อ `actor.json` ด้านล่างคือตัวอย่างที่ตัดมาบางตอน

        public actors = [
            {
                "id": 1,
                ...
            },
            ...
        ];

3. ใน folder เดียวกัน (`src/pages/home`) ให้เปิดไฟล์ `home.html` แล้วดำเนินการต่อเพื่อให้ได้ผลลัพธ์ต่อไปนี้
4. สร้าง list เพื่อนำข้อมูลในตัวแปร `actors` มาแสดงในหน้า home ให้มีความสวยงามตามสมควร (จะใช้ `card` ช่วยหรือไม่ก็ได้)
5. ข้อมูล `mentalScore` ของแต่ละ `actor` ให้แสดงโดยใช้ `badge` และแสดงสีตามค่าตัวเลขโดยถ้าตัวเลขต่ำกว่า 90 ให้มีสีแบบ `danger` แต่ถ้า 90 ขึ้นไปให้ใช้สีแบบ `secondary`

# Assignment #2

## การเตรียมตัว

1. ใช้คำสั่งต่อไปนี้เพื่อสร้าง tutorial project:

        ionic start workshophome tutorial --v2

* ลองศึกษา source code และเรียกใช้งาน เพื่อดูว่ามีการรับ-ส่งข้อมูลระหว่างหน้าอย่างไร

## รายละเอียด

1. สร้าง ionic 2 application โดยใช้ template `sidemenu` ตั้งชื่อ project ตามใจ สำหรับคำสั่งตัวอย่างตั้งชื่อว่า assignment2

        ionic start assignment2 sidemenu --v2

2. ทำขั้นตอนต่าง ๆ ตาม assignment1 คราวที่แล้ว (Assignment #1)
* สร้างหน้า `person` เพิ่มใน project โดยใช้ คำสั่ง (ใน folder ของ project)

        ionic g page person

* ดำเนินการต่อเพื่อให้รายการในหน้า `home` สามารถกดแล้วมาแสดงรายละเอียดข้อมูลที่กดในหน้า `person` ได้อย่างถูกต้อง

# Preparation for the next week
1. Setup Visual Studio
    * มีคำแนะนำเพิ่มเติมที่ดูได้จาก [Visual Studio Tools](http://blog.ionic.io/visual-studio-tools-for-apache-cordova/?_ga=1.95307038.38020642.1478473497)
2. Setup Android SDK or iOS SDK (might need Mac)
    * ภาพรวมการ setup บนเครื่อง Windows ดูได้จาก [Windows Setup](http://ionicframework.com/docs/v2/resources/platform-setup/windows-setup.html)
    * รายละเอียดของการติดตั้ง Android SDK [Android Platform Guide](https://cordova.apache.org/docs/en/latest/guide/platforms/android/)
    * สำหรับเครื่อง Mac อาจติดตั้ง [iOS Platform Guide](https://cordova.apache.org/docs/en/latest/guide/platforms/ios/)
