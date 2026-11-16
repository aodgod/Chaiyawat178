# California Housing — ML Deployment Lab

เว็บ Flask สำหรับประมาณราคาบ้านมัธยฐานของพื้นที่ในแคลิฟอร์เนีย โดยใช้ Random Forest ที่ฝึกจากชุดข้อมูล California Housing ผลทำนายอยู่ในหน่วย 100,000 ดอลลาร์สหรัฐ

## โครงสร้างโปรเจกต์

```text
ml-deployment-lab/
├── app.py
├── train_model.py
├── requirements.txt
├── Dockerfile
├── templates/
│   └── main.html
└── model/
    └── model.pkl
```

`train_model.py` สร้าง `model/model.pkl` หลังฝึกโมเดล ส่วน Docker จะฝึกโมเดลระหว่าง build จึงต้องใช้อินเทอร์เน็ตในการติดตั้ง dependencies และดาวน์โหลดชุดข้อมูลครั้งแรก ไฟล์โมเดลถูกละเว้นจาก Git เพราะสร้างซ้ำได้

## รันด้วย Docker Desktop

เปิด Docker Desktop และเปิด Terminal ในโฟลเดอร์ที่มี Dockerfile

```powershell
docker build -t ds-ml-service .
docker run --rm -p 5000:5000 ds-ml-service
```

เมื่อ Terminal แสดง `Listening at: http://0.0.0.0:5000` ให้เปิด [http://127.0.0.1:5000/](http://127.0.0.1:5000/) แล้วกด **Use sample values** และ **Predict** ค่าทดสอบที่ได้จากโมเดลคือประมาณ 4.039 หรือ 403,899 ดอลลาร์สหรัฐ

เปิด Terminal อีกแท็บเพื่อดู Docker image

```powershell
docker images ds-ml-service
```

## ทดสอบ API ด้วย PowerShell

```powershell
Invoke-RestMethod -Uri http://127.0.0.1:5000/health
Invoke-WebRequest -Uri http://127.0.0.1:5000/predict -Method POST -ContentType 'application/json' -Body '{"features":[8.3,25,6,1,1200,3,34.2,-118.3]}'
```

ลำดับฟีเจอร์คือ MedInc, HouseAge, AveRooms, AveBedrms, Population, AveOccup, Latitude และ Longitude

## สร้างไฟล์โมเดลในเครื่องสำหรับภาพโครงสร้าง

รันใน PowerShell จากโฟลเดอร์โปรเจกต์

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe train_model.py
```

เปิด VS Code แล้วขยายโฟลเดอร์ `model` และ `templates` เพื่อให้เห็น `model.pkl` และ `main.html`

## หลักฐานที่ใช้ส่งงาน

1. ภาพโครงสร้างไฟล์ใน VS Code
2. ภาพ Terminal ที่รัน Docker container สำเร็จและมีข้อความ Listening at หากช่องนี้ให้แนบทั้ง Terminal และหน้าเว็บ ให้จัดหน้าต่างทั้งสองไว้ในภาพเดียว
3. ภาพเบราว์เซอร์ที่เห็น URL และผลหลังคลิก Predict
4. ภาพผลคำสั่ง `docker images ds-ml-service`
5. ภาพการทดสอบ API เป็นรายการเสริม

อัปโหลดไฟล์โปรเจกต์และ README ไปยัง GitHub repository ของผู้ส่งงาน จากนั้นนำลิงก์ repository ของผู้ส่งงานไปกรอกใน [แบบฟอร์มส่งงาน](https://forms.gle/D8UCryawXbEn4FSe7)

อ้างอิงโจทย์และโค้ดตั้งต้นจาก [mssrisupang/ml-deployment-lab](https://github.com/mssrisupang/ml-deployment-lab)

ในโฟลเดอร์ `evidence/` มีภาพโครงสร้างจากคำสั่ง `tree` และภาพผลทำนายจากการรัน Flask ผ่าน Gunicorn โดยตรง สำหรับแบบฟอร์มที่ต้องการ URL ในแถบที่อยู่และหลักฐาน Docker ให้จับภาพใหม่หลังรันด้วย Docker Desktop ตามขั้นตอนข้างต้น
