2. Створити невеличку БД з 5 документів з використанням полів різних типів
   (зокрема, масивів та вкладених документів). Виконати оновлення даних,
   добавлення нових записів, використовуючи веб-інтерфейс Fauxton.

   `   {
   "_id": "ac7",
   "_rev": "1-7ad5f66f9da53832797cfe4bd8e760d6",
   "name": "AC7 Phone",
   "brand": "ACME",
   "type": "phone",
   "price": 320,
   "rating": 4,
   "warranty_years": 1,
   "available": false
   }`
3. Написати простий запит за допомогою Mango Query.

   `   {
   "selector": {
   "price": {
   "$gt": 12
   }
   }
   }`
4. Написати представлення з використанням Map-функції для пошуку записів за
   певними полями.

   `function (doc) {
   if ("limits" in doc) {
   for (const limitKey in doc.limits) {
   const key = doc.name + " -- " + limitKey;
   emit(key, doc.limits[limitKey]);
   }
   }
   }`
5. Написати представлення з використанням Map-функції для пошуку записів за
   певними полями і виконати додатково операцію агрегування по сформованих
   результатах (наприклад, сумування).
   `function (doc) {
   if ("limits" in doc) {
   for (const limitKey in doc.limits) {
   let limitValue = doc.limits[limitKey].n;
   limitValue = limitValue === "unlimited" ? 100000: limitValue;
   emit(doc.name, limitValue)
   }
   }
   }`

   `curl http://admin:pass@localhost:5984/products/_design/limits/_view/limits-average?group=true`
6. Виконати реплікацію створеної БД.
7. За допомогою cURL задати PUT-запит, який помістить в створену БД новий
   документ з конкретним _id за Вашим вибором.

   `curl -X PUT http://admin:pass@localhost:5984/products/507d95d5719dbef170f15c00 -H "Content-type: application/json" -d '{ "name" : "Phone Service Family Plan", "type" : "service", "monthly_price" : 90,"rating" : 4, "limits" : { "voice" : { "units" : "minutes", "n" : 1200, "over_rate" : 0.05 }, "data" : { "n" : "unlimited", "over_rate" : 0 }, "sms" : { "n" : "unlimited", "over_rate" : 0 } }, "sales_tax" : true, "term_years" : 2 }'`
8. Проробити команди створення та знищення нової БД за допомогою cURL.
   
   `curl -X PUT http://admin:pass@localhost:5984/test`

   `curl -X DELETE http://admin:pass@localhost:5984/test`
9. допомогою cURL створити новий документ, який буде містити текстовий
   документ-вкладення. Виконати запит, який поверне цей документ-вкладення.

   `curl -X PUT http://admin:pass@localhost:5984/test/1 -d '{"name": "Attachment"}'`

   `curl -X PUT http://admin:pass@localhost:5984/test/1/doc.txt?rev=1-8fa2ec940be9beec04651e22a7dc4874 -H "Content-type: text/plain" --data-binary @assets/sample.txt`

   `curl  http://admin:pass@localhost:5984/test/1`

   `curl  http://admin:pass@localhost:5984/test/1?attachments=true`
