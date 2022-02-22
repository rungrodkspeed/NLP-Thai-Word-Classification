### การจำแนกความหมายของคำว่า "ตา" ภายในประโยค 
-------------
คำว่า ตา ในภาษาไทยมีหลายความหมายไม่ว่าจะเป็น คุณตา ที่หมายถึงตัวบุคคล, ตาที่แล้ว ที่หมายถึงคราว และ ตาที่หมายถึงอวัยวะสำหรับการมองเห็น ดังนั้นหากเราสามารถทำให้เครื่องคอมสามารถรับรู้ถึงความหมายที่หลากหลายได้ก็จะสามารถนำไปประยุกต์กับคำอื่นๆ หรืองานต่างๆ ได้

### Data
-------------
ภายในไฟล์ .txt จะประกอบไปได้ในส่วนของ data ซึ่งเป็นประโยคที่มีคำว่า "ตา" อยู่ภายในประโยค ซึ่งในแต่ละประโยคสามารถมีคำว่า "ตา" กี่คำก็ได้ ยกตัวอย่างเช่น
- คนมันจะไปน้ำตาแค่ไหนก็รั้งไม่อยู่
- ทุกๆครั้งที่ตาของผมจ้องมองคุณผมแทบบ้า
- ตานี้มีไว้เพื่อมองเธอ
- ตาที่แล้วฉันชนะ ตานี้ก็ด้วย แต่คุณตาบอกว่าฉันโกง

และในส่วนของ labels จะกำกับความหมายของคำว่า "ตา" แต่ละประเภท คือ E ที่หมายถึง eyes ซึ่งหมายถึงอวัยวะที่ใช้ในการมองเห็น, T ที่หมายถึง time ซึ่งหมายถึงคราว และ P ที่หมายถึง person ซึ่งหมายถึงตัวบุคคล ยกตัวอย่างเช่น
- เพราะตาที่แล้วฉันอวดไว้เยอะ ตานี้ฉันจะแพ้คุณตาไม่ได้  จะมี label คือ T, T, P ตามลำดับ



### Preprocessing Data
-------------
- กำจัดส่วนที่เป็นช่องว่างของประโยค ของทั้ง dataset
- ตัดคำภายในประโยคให้ออกเป็นคำๆ โดยใช้ deepcut
- tag คำต่างๆ ของคำว่า "ตา" ให้อยู่ในรูปแบบของ labels โดยที่คำอื่นๆ ที่ไม่ใช่คำว่า "ตา" เราจะไม่สนใจ โดย tag เป็น "."

เมื่อทำเสร็จแล้วจะได้รูปแบบของ data ดังภาพ
![](/blob/processing.png)

จากนั้นทำข้อมูลดังกล่าวไปเข้ารหัสอีกทีจะได้ข้อมูล ดังภาพ
![](/blob/encode_data.png)

แต่เนื่องจากแต่ละประโยคภายใน dataset มีจำนวนคำไม่เท่ากัน ดังนั้นเราจำเป็นต้อง pading ในส่วนของ data ให้มีจำนวนคำที่เท่ากัน โดยเราจะเพิ่มจำนวนคำให้เป็นค่า max ของประโยคที่มีคำมากที่สุดใน dataset ดังภาพ
![](/blob/pad_data.png)

หลังจากที่ผ่านกระบวนการทั้งหมดแล้ว เราก็จะแปลงคำใดๆ ให้เป็นเวกเตอร์เพื่อให้สามารถนำมาคำนวณทางคณิตศาสตร์ได้ โดยเราจะใช้ model Word2Vec จาก pythainlp ในการเปลี่ยนคำให้กลายเป็นเวกเตอร์

### Model
----
ในส่วนของการเรียนรู้เพื่อเครื่องคอมเกิดการรู้จำได้นั้น เราจะใช้ Bidirectional LSTM Neural Network
เพื่อหลีกเลี่ยงปัญหา vanishing gradient ของ Recurrent Neural Network
โดยประกาศโครงสร้างดังนี้
![](/blob/structure.png)

และผลลัพธ์จากการเทรนด์
![](/blob/accuracy.png)

### Result
----
กำหนดประโยคในการทดสอบ model คือ
		"ขอตาหน้าอีกตาเดียว" กูเห็นมึงพูดแบบนี้มาทุกตา 555+
	 การเดินหมากตานี้ทำให้ผมมองโกะในมุมใหม่
	 การที่ลูกได้รดน้ำดำหัวปู่ย่าตายาย จะส่งเสริมพัฒนาการด้านใดบ้าง
	 การปิดตาตีหม้อนิยมเล่นกันมานานแล้ว โดยเฉพาะในเทศกาลวันสงกรานต์เพราะเป็นการละเล่นที่ชวนให้สนุกทั้งคนเล่นและคนดู ฝึกความจำ การสังเกตทิศทางและการกะระยะทาง
	 การผ่าตัดเปลี่ยนกระจกตาสำเร็จเป็นครั้งแรกนั้นไม่ใช่ตาของคนเรา แต่เป็นตาของสัตว์
	 การมีสุขภาพตาและการมองเห็นที่ดี เป็นส่วนหนึ่งของการมีคุณภาพชีวิตที่ดี

และเมื่อให้ตัว model ทำนายประโยคเหล่านี้จะได้ผลลัพธ์ดังภาพ
![](/blob/result.png)
