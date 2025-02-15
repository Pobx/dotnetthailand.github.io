---
title: Docker network
showMetadata: true
editable: true
showToc: true
---

# ประเภทของเครือข่ายในด็อกเกอร์

เครือข่ายเสมือนในด็อกเกอร์ทำงานโดยใช้ไดร์เวอร์ หรือซอฟต์แวร์เป็นตัวขับเคลื่อนลักษณะการทำงาน โดยค่าเริ่มต้นมีอยู่ 5 ประเภท  คือ
- บริดจ์ (Bridge) เป็นไดร์เวอร์เครือข่ายเริ่มต้น หากไม่ได้ระบุตอนสร้างเครือข่ายเสมือน เครือข่ายบริดจ์ของด็อกเกอร์มีไว้สำหรับแอปพลิเคชันที่ในคอนเทนเนอร์เดี่ยวสามารถเชื่อมต่อกันได้ ไดร์เวอร์นี้เหมาะสมหากต้องการให้คอนเทนเนอร์สามารถเชื่อมต่อเครื่องภายนอกหรืออินเทอร์เน็ต
- โฮสต์ (Host) เป็นไดร์เวอร์เครือข่ายที่เชื่อมต่อเฉพาะคอนเทนเนอร์กับโฮสต์เท่านั้น ทำให้
คอนเทนเนอร์ไม่สามารถเชื่อมต่อกับเครื่องภายนอก หรืออินเทอร์เน็ตได้โดยตรง หากต้องการเชื่อมต่อเครื่องภายนอก หรืออินเทอร์เน็ต คอนเทนเนอร์ที่มีอินเตอร์เฟสเครือข่ายที่ใช้ไดร์เวอร์นี้จะได้เชื่อมต่อผ่านคอนเทนเนอร์อื่นที่มีไดร์เวอร์อินเตอร์เฟสเครือข่ายเป็นบริดจ์
- โอเวอร์เลย์ (Overlay) เป็นไดร์เวอร์ที่เชื่อมเครื่องคอมพิวเตอร์ที่ติดตั้งด็อกเกอร์โฮสต์หลายเครื่องเข้าด้วยกัน ทำให้ในการเชื่อมต่อผ่านไดร์เวอร์นี้ ด็อกเกอร์โฮสต์ในคอมพิวเตอร์ทุกเครื่องทำงานเสมือนหนึ่งเป็นเครื่องเดียวกัน ดังนั้น คอนเทนเนอร์ที่ทำงานอยู่บนต่างด็อกเกอร์โฮสต์จะสามารถเชื่อมต่อกันได้เสมือนหนึ่งว่าคอนเทนเนอร์นั้นทำงานอยู่บนเครื่องด็อกเกอร์โฮสต์เดียวกัน ไดร์เวอร์นี้จะถูกสร้างโดยอัตโนมัติเพื่อด็อกเกอร์โฮสต์หลายเครื่องสร้างด็อกเกอร์สวอร์ม (Docker Swarm) (Docker Inc., 2020c) ร่วมกันเท่านั้น
- แมควีแลน (Macvlan) เป็นไดร์เวอร์ที่อนุญาตให้ระบุที่อยู่ทางกายภาพของอินเตอร์เฟสเครือข่าย (MAC address) ให้กับอินเตอร์เฟสเครือข่ายของคอนเทนเนอร์ได้ ทำให้คอนเทนเนอร์ใช้ที่อยู่ไอพีเดียววงเดียวกันกับด็อกเกอร์โฮสต์ได้ ด้วยการสร้างเครือข่ายบริดจ์ (Bridge network) กับอินเตอร์เฟสเครือข่ายของด็อกเกอร์โฮสต์  แต่การส่งข้ามข้อมูลระหว่างเครื่องภายนอกกับ
คอนเทนเนอร์ทั้งหมดที่ใช้ไดร์เวอร์นี้จะเชื่อมต่อกันด้วยที่อยู่ทางกายภาพของอินเตอร์เฟสเครือข่ายของด็อกเกอร์โฮสต์เดียวกัน ดังนั้น อุปกรณ์เครือข่ายหรือสวิตซ์จำเป็นต้องสนับสนุนการทำงานแบบ Promiscuous mode ด้วย
- ไม่มี (None) เป็นการระบุว่ายกเลิกการทำงานของไดร์เวอร์เครือข่ายเพื่อไม่ให้คอนเทนเนอร์นั้นสามารถเชื่อมต่อกับคอนเทนเนอร์อื่นได้
แม้จะมีไดร์เวอร์หลายรูปแบบในด็อกเกอร์ให้เลือกใช้ อย่างไรก็ตาม ไดร์เวอร์ที่นิยมถูกใช้งานและเหมาะในสถานการณ์ส่วนใหญ่คือการใช้ไดร์เวอร์บริดจ์ เพราะด็อกเกอร์จะสร้างเครือข่ายอีกวงหนึ่งขึ้นมาให้
คอนเทนเนอร์ได้ปฏิสัมพันธ์กันโดยข้อมูลสามารถวิ่งผ่านด็อกเกอร์โฮสต์ไปยังเครือข่ายภายนอกทันที และ
เครื่องคอมพิวเตอร์ภายนอกสามารถเชื่อมต่อผ่านพอร์ตของด็อกเกอร์โฮสต์ โดยที่ประสิทธิภาพของเครือข่ายแบบบริดจ์มีประสิทธิภาพใกล้เคียงกับการเชื่อมต่อกับแอปพลิเคชันที่ทำงานโดยไม่ได้ผ่านด็อกเกอร์โฮสต์

# List all networks

```
docker network ls
```

# Remove a network

```
docker network rm network-name
```

#

# Example of using host network

- !!! The host networking driver only works on Linux hosts, and is not supported on Docker Desktop for Mac, Docker Desktop for Windows, or Docker EE for Windows Server.
- Run Nginx from localhost:80 but other resources isolated from a host

```
docker run --rm -d --network host --name my_nginx nginx
```

- Access Nginx by browsing to http://locahost:80/.
- More details https://docs.docker.com/network/network-tutorial-host/
- เอกสารภาษาไทยตำรา [รายวิชา การประมวลผลกลุ่มเมฆ โดย ผศ.ดร.อุทาน บูรณศักดิ์ศรี](https://www.scribd.com/document/478584064/%E0%B8%95%E0%B8%B3%E0%B8%A3%E0%B8%B2%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B8%9B%E0%B8%A3%E0%B8%B0%E0%B8%A1%E0%B8%A7%E0%B8%A5%E0%B8%9C%E0%B8%A5%E0%B8%81%E0%B8%A5%E0%B8%B8-%E0%B8%A1%E0%B9%80%E0%B8%A1%E0%B8%86-Cloud-Computing)
