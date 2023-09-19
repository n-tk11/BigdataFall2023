## การบ้าน Bigdata kafka 
แก้ code ที่ process ว่า เมนูไหนมี calories เท่าไหร่ และเกิน threshhold ไหน โดยแกะจาก html จากเว็บ
แก้เฉพาะไฟล์ *test_kafka_prod_consumer_new.ipynb*
ให้ function parse เอา html ของ recipe มาแกะดู ingredient (ใช้ BeutifulSoup) คำนวน calories มาส่งไปวิเคราะห์ต่อ
1. แกะ html หา list ingredient ตัวอย่าง code
```python
ingredient_ul = soup.find('ul', class_='ingredient')
        if ingredient_ul:
            # Find all <li> elements within the <ul> element
            ingredients_section = ingredient_ul.find_all('li')
```
2. เอา  gredient ไปส่ง api หา calories ตัวอย่าง code 
```python
def get_calories(query):
    #print('query=',query)
    api_url = 'https://api.api-ninjas.com/v1/nutrition?query={}'.format(query)
    response = requests.get(api_url, headers={'X-Api-Key': 'G0JUdMfBTM9fB1awvtBxfQ==0fwOwsEexgpkHqi7'})
    if response.status_code == requests.codes.ok:
        jsonResponse = response.json()
        sum = 0 
        for item in jsonResponse:
            sum += item['calories']
        return sum
    else:
        print("Error:", response.status_code, response.text)
        return 0
```
ปล. api-key ต้องไปสมัคร account api-ninjas ก่อน
ปล2. ก่อนรัน start zookeeper/kafka server บนทั้ง 2 เครื่องก่อน
eg. 
```shell
bin/zookeeper-server-start.sh config/zookeeper.properties &
bin/kafka-server-start.sh config/server.properties  &
```
และก็รัน notebook *test_kafka_producer_new.ipynb* ตามด้วย
*test_kafka_prod_consumer_new.ipynb*

sources ตั้งต้น
- https://www.cpe.ku.ac.th/~cnc/downloads/test_kafka_producer_new.ipynb
- https://www.cpe.ku.ac.th/~cnc/downloads/test_kafka_prod_consumer_new.ipynb
