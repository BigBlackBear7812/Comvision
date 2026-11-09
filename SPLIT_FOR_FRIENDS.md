# คำสั่งแยกงานกลุ่ม — ส่งไฟล์นี้ไปกับโฟลเดอร์ทั้งก้อน

อ่านไฟล์นี้ทั้งไฟล์ก่อนลงมือ โปรเจกต์คือชุดเครื่องมือ MATLAB หัวข้อ **13 Creating Augmented Reality Filters** อยู่ในโฟลเดอร์ `AR_Filters_Toolbox`

งานทั้งก้อนทำครบแล้ว ห้ามเขียนอัลกอริทึมใหม่ ห้ามเปลี่ยนสมการ ห้ามสุ่มตัวเลขผลทดลองใหม่ สิ่งที่ต้องทำคือ **แยกของที่มีอยู่** เป็นชุดส่งของแต่ละคน

ชื่อในไฟล์ยังเป็น Student 1, Student 2, Student 3 ให้ใช้ชื่อและรหัสที่ผู้ใช้พิมพ์ให้แทนคำเหล่านี้ทุกจุดที่แยกแล้ว ถ้าผู้ใช้ยังไม่ให้ชื่อจริง ให้คงคำว่า Student 1 / 2 / 3 ไว้ และอย่าแต่งชื่อคนขึ้นเอง

---

## สิ่งที่ต้องสร้าง

สร้าง 3 โฟลเดอร์ คัดลอกไฟล์ต้นฉบับเข้าไป อย่าแก้ไฟล์ในโฟลเดอร์เดิม

```
submission_student1/
submission_student2/
submission_student3/
```

แต่ละโฟลเดอร์มี 4 ส่วน

```
code/          ไฟล์ .m ที่คนนี้รับผิดชอบ + ไฟล์ร่วมที่โค้ดเขาเรียกใช้
figures/       รูปผลของส่วนเขา
report/        รายงานเฉพาะส่วนเขา เป็น .docx ถ้าทำได้ ไม่งั้นเป็น .md
slides/        สไลด์เฉพาะส่วนเขา
README.txt     บอกว่าเปิดอะไร และส่วนที่เหลือเป็นของเพื่อนคนไหน
```

รายงานต้นฉบับทั้งเล่มคือ `report/Project_Report.docx` สไลด์ทั้งชุดคือ `presentation/Group_Presentation.pptx` เวลาแยกให้นำเฉพาะหัวข้อด้านล่าง ไม่ต้องย่อความจนตัวเลขหาย

ตัวเลขที่ห้ามเปลี่ยน

- ส่วนเบี่ยงเบนมาตรฐานภาพมืด 0.082 หลังอีควอไลซ์ 0.374
- PSNR ภาพรบกวน 16.54 dB, ค่าเฉลี่ย 24.13 dB, มัธยฐาน 35.46 dB
- จุดขอบโซเบล 9539, พรีวิตต์ 9538, แคนนี 3691
- โอสึ t = 128
- ความเป็นวงกลมของวงกลมสองวง 1.250 และ 1.247, สี่เหลี่ยม 0.784, รูปเอียง 0.847
- ตาซ้าย (272, 347), ตาขวา (392, 347), ระยะตา 120 พิกเซล, คลาดเคลื่อน 0
- ใบหน้าเอียง 12 องศา ตัวตรวจได้ 11.03 องศา
- ลงทะเบียนหมุนไว้ 8 องศา ได้กลับ 7.89 องศา, ข้อมูลร่วม 1.054 เป็น 1.991, PSNR ในกรอบ 25.02 dB
- ค่าเลื่อน (33.62, -27.33) ไม่ใช่ค่าเลื่อนบริสุทธิ์ 22 และ -16 เพราะหมุนรอบจุดกำเนิด
- เลนส์ k1 = -0.22, PSNR หลังแก้กลับ 18.33 dB

---

## Student 1 — ปรับปรุงภาพ

หน้าที่: ฮิสโทแกรม, ความสว่างความเปรียบต่าง, ฟิลเตอร์เชิงพื้นที่, ฟิลเตอร์ฟูเรียร์ และจุดที่ฟิลเตอร์เสมือนเรียกตัวกรองเหล่านี้

### โค้ดที่ให้คัดลอกไป `code/`

ของเขาโดยตรง

- `module1_enhancement/histogramEqualize.m`
- `module1_enhancement/histogramMatch.m`
- `module1_enhancement/grayLevelTransform.m`
- `module1_enhancement/spatialMeanFilter.m`
- `module1_enhancement/spatialMedianFilter.m`
- `module1_enhancement/laplacianSharpen.m`
- `module1_enhancement/highPassSharpen.m`
- `module1_enhancement/frequencyFilter.m`
- `ar_filters/beautyFilter.m`
- `ar_filters/backgroundBlur.m`

ไฟล์ร่วมที่โค้ดเขาเรียก ใส่ใน `code/common/` และเขียนใน README ว่าเป็นユーทิลิตี้ร่วม ไม่ใช่ผลงานหลักของเขา

- `common/toDouble.m`
- `common/toUint8Image.m`
- `common/asGray.m`
- `common/applyPlane.m`
- `common/gaussKernel.m`
- `common/rgb2ycbcrLocal.m`
- `common/ycbcr2rgbLocal.m`

`beautyFilter.m` เรียก `sobelEdge.m` และ `backgroundBlur.m` เรียก `morphOperation.m` กับ `detectFaceRegions.m` ของ Student 2 ให้คัดลอกไฟล์ที่ต้องมีเพื่อให้รันได้ไปไว้ `code/used_from_teammates/` และเขียนชัดว่าไม่ใช่โค้ดของ Student 1

### รูปที่ให้คัดลอกไป `figures/`

- `results/m1_histogram.png`
- `results/m1_graylevel.png`
- `results/m1_denoise.png`
- `results/m1_sharpen.png`
- `results/m1_frequency.png`
- `results/ar_beauty.png`
- `results/ar_bgblur.png`
- `samples/portrait.png`
- `samples/portrait_dark.png`
- `samples/portrait_noise.png`

### รายงาน

จาก `report/Project_Report.docx` เอาเฉพาะส่วนนี้ไปทำรายงานของเขา

- ปกย่อ ใส่ชื่อเขาเป็นผู้จัดทำส่วนปรับปรุงภาพ และระบุชื่อเพื่อนอีกสองคนเป็นผู้ร่วมกลุ่ม
- บทคัดย่อสั้นเฉพาะโมดูล 1 กับจุดที่ฟิลเตอร์เรียกตัวกรองของเขา
- หัวข้อ 4 ทั้งหัวข้อ (4.1 ฮิสโทแกรม, 4.2 ฟิลเตอร์เชิงพื้นที่, 4.3 โดเมนความถี่)
- ในหัวข้อ 7 เอาเฉพาะย่อหน้าที่พูดเรื่องโหมดความงาม โหมดเบลอฉากหลัง และลำดับอีควอไลซ์หลังตรวจตา
- ในหัวข้อ 9 เอาเฉพาะปัญหาที่อีควอไลซ์ก่อนตรวจสีผิวแล้วแว่นไปอยู่ที่บอลลูน
- ภาคผนวกเฉพาะรายชื่อฟังก์ชันของ Student 1

อย่านำหัวข้อขอบภาพ โอสึ จุดตา การหมุน หรือ GUI ไปเป็นเนื้อหลักของเขา

### สไลด์

จาก `presentation/Group_Presentation.pptx` มี 11 สไลด์ตามลำดับนี้

1. ฟิลเตอร์เสมือนบนชุดเครื่องมือ MATLAB
2. ใครทำอะไร
3. โมดูล 1 ปรับภาพก่อนวางฟิลเตอร์
4. โมดูล 2 ขอบและวัตถุ
5. จุดยึดของฟิลเตอร์มาจากสีผิวและตา
6. โมดูล 3 แมปย้อนกลับไม่มีรู
7. ลงทะเบียนภาพและแก้เลนส์บาร์เรล
8. วางแว่น หมวก หนวด ให้ตรงตา
9. ฟิลเตอร์ที่ใช้โมดูล 1 และ 2
10. สิ่งที่ยังทำไม่ได้ดี
11. สรุป

ของ Student 1 คือสไลด์ 3 ทั้งแผ่น และสไลด์ 9 เฉพาะสองข้อแรก (ความงาม = มัธยฐาน, ฉากหลัง = โลว์พาส) สไลด์ 1 และ 2 ใส่ไว้หน้าชุดของเขาได้ แต่โน้ตคำพูดของสไลด์ 3 ต้องเป็นส่วนที่เขาพูดประมาณ 80 วินาที

---

## Student 2 — แบ่งส่วน ขอบภาพ และจุดบนใบหน้า

หน้าที่: โซเบล พรีวิตต์ แคนนี โอสึ การโตของบริเวณ สี HSV มอร์โฟโลยี ความเป็นวงกลม หน้ากากสีผิว และจุดตา

### โค้ดที่ให้คัดลอกไป `code/`

- `module2_segmentation/sobelEdge.m`
- `module2_segmentation/prewittEdge.m`
- `module2_segmentation/cannyEdge.m`
- `module2_segmentation/compareEdgeDetectors.m`
- `module2_segmentation/otsuThreshold.m`
- `module2_segmentation/regionGrowing.m`
- `module2_segmentation/colorSegmentation.m`
- `module2_segmentation/morphOperation.m`
- `module2_segmentation/shapeSegmentation.m`
- `module2_segmentation/connectedComponents.m`
- `module2_segmentation/skinMask.m`
- `ar_filters/detectFaceRegions.m`
- `ar_filters/colorPop.m`

ไฟล์ร่วม ใส่ `code/common/` และบอกว่าเป็นของร่วม

- `common/toDouble.m`
- `common/asGray.m`
- `common/diskSE.m`
- `common/gaussKernel.m`
- `common/rgb2ycbcrLocal.m`

`cannyEdge.m` เรียก `connectedComponents.m` ซึ่งเป็นของเขาอยู่แล้ว `detectFaceRegions.m` เรียก `skinMask.m` และ `morphOperation.m` ซึ่งก็เป็นของเขา

### รูปที่ให้คัดลอกไป `figures/`

- `results/m2_edges.png`
- `results/m2_otsu_shape.png`
- `results/m2_region_skin.png`
- `results/m2_color_morph.png`
- `results/ar_landmarks.png`
- `results/ar_colorpop.png`
- `samples/portrait.png`
- `samples/house.png`
- `samples/shapes.png`

### รายงาน

- ปกย่อ เขาเป็นผู้จัดทำส่วนแบ่งส่วนและตรวจใบหน้า
- หัวข้อ 5 ทั้งหัวข้อ (5.1 ถึง 5.4)
- ในหัวข้อ 7 เอาเฉพาะย่อหน้าที่พูดเรื่องหน้ากากสีผิว จุดตา มุมจาก atan2 เฉพาะฝั่งที่มาจากตัวตรวจ และโหมดคงสีแดง
- ในหัวข้อ 9 เอาปัญหาบอลลูนแดงหลุดเข้ากฎ RGB, แถวตาที่ต้องวัดความกว้าง, โอสึที่เกณฑ์แรกเป็น 0, และความเป็นวงกลมที่ต้องนับขอบแบบสี่เพื่อนบ้าน
- ในหัวข้อ 10 เอาข้อจำกัดเรื่องช่วง Cb Cr, ใบหน้าด้านข้าง, แว่นทึบ และแคนนีที่เส้นโค้งขาด
- ภาคผนวกรายชื่อฟังก์ชันของ Student 2

### สไลด์

ของเขาคือสไลด์ 4 และ 5 ทั้งแผ่น และสไลด์ 9 เฉพาะข้อคงสีแดงกับประโยคว่าต้องตรวจตาก่อนแล้วค่อยอีควอไลซ์ คำพูดสไลด์ 4 ประมาณ 60 วินาที สไลด์ 5 อีกประมาณ 60 วินาที ต้องพูดเองว่าวงกลมของแคนนียังไม่ต่อ เพราะเกรเดียนต์บางมุมต่ำกว่าเกณฑ์ฮิสเทอรีซิส

---

## Student 3 — เรขาคณิต การวางฟิลเตอร์ และ GUI

หน้าที่: เลื่อน ย่อ หมุน แอฟฟีน เลนส์ อินเทอร์โพเลชัน ลงทะเบียนภาพ วาง PNG โปร่งใส รวม GUI และไฟล์เปิดโปรแกรม

### โค้ดที่ให้คัดลอกไป `code/`

- `module3_geometry/resampleImage.m` อยู่ที่ `common/resampleImage.m` ให้ใส่ในชุดเขา เพราะเป็นตัวประมาณค่าที่เขาเขียน
- `module3_geometry/resizeImage.m`
- `module3_geometry/translateImage.m`
- `module3_geometry/scaleImage.m`
- `module3_geometry/rotateImage.m`
- `module3_geometry/affineWarp.m`
- `module3_geometry/shearMatrix.m`
- `module3_geometry/radialDistortion.m`
- `module3_geometry/harrisCorners.m`
- `module3_geometry/registerImages.m`
- `module3_geometry/mutualInformation.m`
- `ar_filters/alphaBlend.m`
- `ar_filters/placeSprite.m`
- `ar_filters/applyARFilter.m`
- `ar_filters/loadSprite.m`
- `ar_filters/filterCatalog.m`
- `ar_filters/drawLandmarks.m`
- `launchToolbox.m`
- `main.m`
- `runDemo.m`
- `selfTest.m`

ไฟล์ร่วมที่ตัวเรขาคณิตเรียก ใส่ `code/common/`

- `common/toDouble.m`
- `common/toUint8Image.m`
- `common/asGray.m`
- `common/gaussKernel.m`

`applyARFilter.m`, `launchToolbox.m`, `runDemo.m` เรียกฟังก์ชันของเพื่อน คัดลอกของเพื่อนไป `code/used_from_teammates/` ทั้งโฟลเดอร์ `module1_enhancement` และ `module2_segmentation` รวม `detectFaceRegions.m`, `beautyFilter.m`, `backgroundBlur.m`, `colorPop.m` เพื่อให้ `main` เปิดได้ แต่ README ต้องบอกว่าอัลกอริทึมในสองโฟลเดอร์นั้นเป็นของ Student 1 และ Student 2

คัดลอก `assets/` ทั้งโฟลเดอร์ไปกับเขา เพราะสไปรต์เป็นชิ้นที่ตัววางภาพใช้ และคัดลอก `samples/` ไปด้วยเพื่อให้รัน `selfTest` ได้

### รูปที่ให้คัดลอกไป `figures/`

- `results/m3_geometry.png`
- `results/m3_mapping.png`
- `results/m3_interpolation.png`
- `results/m3_affine_lens.png`
- `results/m3_registration.png`
- `results/ar_filters_a.png`
- `results/ar_filters_b.png`
- `results/ar_glasses.png`
- `results/ar_mustache.png`
- `results/ar_hat.png`
- `results/ar_party.png`
- `results/ar_dog.png`
- `results/ar_combo.png`
- `results/gui_layout.png`
- `samples/portrait.png`
- `samples/portrait_moving.png`
- `samples/grid.png`
- `assets/glasses.png`
- `assets/mustache.png`
- `assets/hat.png`
- `assets/party.png`
- `assets/dog_ears.png`
- `assets/dog_nose.png`

### รายงาน

- ปกย่อ เขาเป็นผู้รวมระบบ
- หัวข้อ 3 โครงสร้างระบบ
- หัวข้อ 6 ทั้งหัวข้อ
- หัวข้อ 7 เฉพาะการวางสไปรต์ มุมที่ต้องใส่เครื่องหมายลบ ความกว้างที่คิดจากระยะตา และลำดับวาดหมวกแล้วแว่นแล้วหนวด
- หัวข้อ 8 การใช้งาน GUI ทั้งหัวข้อ
- ในหัวข้อ 9 เอาปัญหาแว่นเอียงผิดทิศ และเลนส์ที่บิดแรงจนขอบดำ
- ในหัวข้อ 10 เอาข้อจำกัดว่าการลงทะเบียนยังไม่รองรับคนละสเกล และตัวเลขการเลื่อนปนจุดหมุน
- ภาคผนวก ข คำพูดนำเสนอ ให้เหลือช่วงที่ Student 3 พูด คือช่วงเปิด ช่วงเรขาคณิตและเดโมฟิลเตอร์ และช่วงปิด
- ภาคผนวกรายชื่อฟังก์ชันของ Student 3

### สไลด์

ของเขาคือสไลด์ 1, 6, 7, 8 และ 11 สไลด์ 2 ใส่ได้เพราะเป็นหน้าแบ่งหน้าที่ โน้ตสไลด์ 8 คือจังหวะเดโมแว่น หมวก และชุดรวม โน้ตสไลด์ 11 มีคำถามที่เขาควรตอบเรื่องการลงทะเบียนและเรื่องที่ไม่ใช้ `histeq` หรือ `edge` สำเร็จรูป

---

## ของที่ใช้ร่วม ทั้งสามคนอ้างได้ แต่ห้ามคนเดียวอ้างว่าเขียนคนเดียว

- `common/toDouble.m`, `toUint8Image.m`, `asGray.m`, `gaussKernel.m`, `diskSE.m`, `applyPlane.m`, `rgb2ycbcrLocal.m`, `ycbcr2rgbLocal.m`
- `samples/` ทั้งโฟลเดอร์
- `HOW_TO_RUN.txt`
- สไลด์ 2, 10 และส่วนสรุปของรายงานหัวข้อ 11
- สไลด์ 10 ใครก็พูดได้ คนละประเด็นตามข้อจำกัดของโมดูลตัวเอง

---

## รูปแบบ README.txt ในแต่ละชุด

เขียนสั้น ๆ สี่บรรทัดนี้ให้ครบ โดยเปลี่ยนชื่อคน

```
ผู้จัดทำส่วนนี้: <ชื่อจริง รหัส>  (Student N)
โมดูล: <ชื่อโมดูล>
วิธีลอง: เปิด MATLAB แล้ว cd เข้า code จากนั้นเรียกฟังก์ชันตามรายงาน
ส่วนที่ไม่ได้ทำเอง: ดูโฟลเดอร์ used_from_teammates ถ้ามี
```

---

## สิ่งที่ห้ามทำ

- ห้ามรวมโค้ดทั้งโปรเจกต์แล้วเขียนหน้าปกว่าคนเดียวทำทั้งหมด
- ห้ามลบคอมเมนต์หัวฟังก์ชันที่บอกเลข Student
- ห้ามเปลี่ยนชื่อฟังก์ชัน เพราะรายงานอ้างชื่อเหล่านี้
- ห้ามสร้างผลภาพใหม่แล้วใส่ตัวเลขคนละชุดกับรายการด้านบน
- ห้ามใส่ `tools/build_results.py` หรือ `tools/build_documents.py` เข้าชุดส่งของนักศึกษา ไฟล์นั้นเป็นตัวสร้างรูปและเอกสาร ไม่ใช่ชิ้นงาน MATLAB ที่ต้องส่ง
